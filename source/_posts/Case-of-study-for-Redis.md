title: "TechForce: A case study of Redis"
date: 2013-04-22 18:15:23
description: "A case for studing Redis. This would be the simple
tutorial for better understand the buiding trial."
category: tech 
tags: [tech redis]
---

Read [Theplant Blog](http://theplant.jp/en/blogs/12-techforce-a-case-study-of-redis)

## TechForce: A case study of Redis

`Redis` is one of the open source projects I'm currently most interested in.As a key-value store, Redis has the abillity to store data via its datatypes. In this article I will try to show the process of how we design and implement a simple clone of Twitter written using Ruby and Redis as only database.

Here is a screenshot of the application:

![](http://theplant.jp/system/photos/bgwekoq0t9.png?1345691160)

Our Twitter case is structurally simple. It mainly contains three features for the user: add friends, remove friends and post a tweet. We have only five people in our "world" and each person can get others' tweets by adding them as friends.

Through this simple application, we will learn how to construct a key and use appropriate data types to store values.

Now, let me walk you through the main concepts of this application.

## RubyGem about Redis

In this application, I use "redis-rb" gem, a Ruby client library for
Redis, to match Redis’ API one-to-one while still providing an idiomatic
interface. It features thread-safety, client-side sharding, pipelining,
and an obsession for performance. Here’s the link to visit this project: [Redis][https://github.com/redis/redis-rb]

You can connect to Redis by instantiating the 'Redis' class:

    require  "redis"
    redis = Redis.new(:host => "127.0.0.1", :port => 6379)

## Data Structure

Design a table to store values is easy for a relational database. But We don't have tables now, so we should designed structure to store a value and identify what keys are needed to represent our objects.

Let's start from Users. We need to represent these users of course, with the username, userid, followers and following users, and so on. Like relational databases, it’s a good solution to associate a unique ID to every user to identify users inside our system. Every other reference to this user will be made by id.

    INCR UserID => 1000
    SET user:id:1000:username theplant
    SET username:theplant:user:id 1000

### The core code for constructing Users:

    def init_user(redis)
      redis.set("user:uid", 1000)
      username = %W[Eric Yuan Binku Lancee Juice]
      username.each do |name|
        user_id = Redis.current.incr("user:uid") 
        redis.set("user:id:#{user_id}:username", name)
        redis.set("username:#{name}:user:id", user_id)
        redis.rpush("users", user_id)
      end
    end

## Following, followers

Redis data type "Set" is a perfect data structure to save these values. Every user has followers and users that they are following.

    User:id:1000:followers => Set of ids of followers of current user
    User:id:1000:following => Set of ids of followees of current user

## The core code for the above:

    def add_friend
      redis.sadd("user:id:#{params[:self_id]}:followees", params[:followee_id])
      redis.sadd("user:id:#{params[:followee_id]}:followers", params[:self_id] )
    end

## Post

The most important part of our case is to add the post to every user and display it in the user’s page. Not only saving your own posts but also getting your friends’ tweets on your page. Using the same strategy for saving User data, we also associate a unique ID to every user to identify users inside our system.

    INCR post:id => 1
    SET post:1 "My first tweet!"

Then we use the Redis data type "List" to save every post_id. Introduction from reids.io:

> Redis Lists are simply lists of strings, sorted by insertion order. It is possible to add elements to a Redis List pushing new elements on the head (on the left) or on the tail (on the right) of the list.

    $ user:id:1000:posts => List of post ids

The core code:

    def add_post
      redis = Redis.current
      post_id = redis.incr("Post:Id")
      redis.set("post:#{post_id}", params[:post])
      redis.rpush("posts", post_id)
      redis.sadd("user:id:#{params[:id]}:posts", post_id)
      redis.smembers("user:id:#{params[:id]}:followers").each do |follower|
        redis.sadd("user:id:#{follower}:mentions", post_id)
      end
    end

![](http://theplant.jp/system/photos/p2smz2953a.png?1345692049)

## Following users

There are more than Lists. Redis also supports Sets that are unsorted collections of elements. It is possible to add, remove, and test for the existence of members and perform intersections between different Sets. If user id 1000 (Eric) wants to follow user id 1001 (Yuan), we can do this with just two SADD:

    SADD user:id:1000:following 1001
    SADD user:id:1001:followers 1000

And remove a friend is much like to following a user. It should be pointed out that we will lose a follower when your follower "removes" you. The core code below:

    def remove_friend
      redis.srem("user:id:#{params[:self_id]}:followees", params[:follower_id])
      redis.srem("user:id:#{params[:follower_id]}:followers", params[:self_id])
      remove_ids= redis.smembers("user:id:#{params[:follower_id]}:posts")
      remove_ids.each do |id|
        redis.srem("user:id:#{params[:self_id]}:mentions", id) 
      end
    end

Here is a screenshot of string value we save in Redis:

![](http://theplant.jp/system/photos/f1y1xkk5fc.png?1345692207)

`TechForce` is a weekly meeting held at The Plant for developers to present and discuss new technologies and projects they are working on. Each week a different developer presents.

