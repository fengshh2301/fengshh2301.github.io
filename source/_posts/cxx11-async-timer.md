---
title: c++11异步定时器
date: 2018-01-29 14:09:09
tags: [c++11,异步,timer]
categories : [贴士]
---

​    c++11提供了丰富的时间和线程操作函数，比如 std::this_thread::sleep, std::chrono::seconds等。利用这些可以很方便的实现一个定时器。 
​    定时器要求在固定的时间异步执行一个操作，比如boost库中的boost::asio::deadline_timer，以及MFC中的定时器。这里，利用c++11的thread, mutex, condition_variable 来实现一个定时器： 
​    定时器要求异步执行任务    ----> 开辟独立的线程 
​    定时器要求能够启动和取消 ----> 提供安全的取消操作，使用互斥量和信号量 
​    定时器要求每个定时时刻到达的时候执行的任务要尽可能节省时间

*以下是实现：*

timer.hpp

```c++
#ifndef _TIMER_H_
#define _TIMER_H_
#include<iostream>
#include<functional>
#include<chrono>
#include<thread>
#include<atomic>
#include<memory>
#include<mutex>
#include<condition_variable>
class Timer{
public:
    Timer() :expired_(true), try_to_expire_(false){
    }
 
    Timer(const Timer& t){
        expired_ = t.expired_.load();
        try_to_expire_ = t.try_to_expire_.load();
    }
    ~Timer(){
        Expire();
    }
 
    void StartTimer(int interval, std::function<void()> task){
        if (expired_ == false){
            return;
        }
        expired_ = false;
        std::thread([this, interval, task](){
            while (!try_to_expire_){
				std::cout << "StartTimer:" << std::this_thread::get_id() << std::endl;
                std::this_thread::sleep_for(std::chrono::milliseconds(interval));
                task();
            }
            {
                std::lock_guard<std::mutex> locker(mutex_);
                expired_ = true;
                expired_cond_.notify_one();
            }
        }).detach();
    }
 
    void Expire(){
        if (expired_){
            return;
        }
 
        if (try_to_expire_){
            return;
        }
        try_to_expire_ = true;
        {
            std::unique_lock<std::mutex> locker(mutex_);
            expired_cond_.wait(locker, [this]{return expired_ == true; });
            if (expired_ == true){
                try_to_expire_ = false;
            }
        }
    }
     
    template<typename callable, class... arguments>
    void SyncWait(int after, callable&& f, arguments&&... args){
 
        std::function<typename std::result_of<callable(arguments...)>::type()> task
            (std::bind(std::forward<callable>(f), std::forward<arguments>(args)...));
        std::this_thread::sleep_for(std::chrono::milliseconds(after));
        task();
    }
    template<typename callable, class... arguments>
    void AsyncWait(int after, callable&& f, arguments&&... args){
        std::function<typename std::result_of<callable(arguments...)>::type()> task
            (std::bind(std::forward<callable>(f), std::forward<arguments>(args)...));
 
        std::thread([after, task](){
			std::cout << "AsyncWait:" << std::this_thread::get_id() << std::endl;
            std::this_thread::sleep_for(std::chrono::milliseconds(after));
            task();
        }).detach();
    }
     
private:
    std::atomic<bool> expired_;
    std::atomic<bool> try_to_expire_;
    std::mutex mutex_;
    std::condition_variable expired_cond_;
};
#endif
```

main.cpp

```c++
#include<iostream>
#include<string>
#include<memory>
#include"timer.hpp"
using namespace std;
void EchoFunc(std::string&& s) {
	std::cout << "test : " << s << endl;
}

int main() {
	Timer t;
	//周期性执行定时任务
	t.StartTimer(1000, std::bind(EchoFunc, "hello world!"));
	std::this_thread::sleep_for(std::chrono::seconds(4));
	std::cout << "try to expire timer!" << std::endl;
	t.Expire();

	//周期性执行定时任务
	t.StartTimer(1000, std::bind(EchoFunc, "hello c++11!"));
	std::this_thread::sleep_for(std::chrono::seconds(4));
	std::cout << "try to expire timer!" << std::endl;
	t.Expire();

	std::this_thread::sleep_for(std::chrono::seconds(2));

	//只执行一次定时任务
	//同步
	t.SyncWait(1000, EchoFunc, "hello world!");
	//异步
	t.AsyncWait(1000, EchoFunc, "hello c++11!");

	//std::this_thread::sleep_for(std::chrono::seconds(2));

	system("pause");
	return 0;
}

```

