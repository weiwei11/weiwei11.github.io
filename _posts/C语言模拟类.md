---
layout: post
title: C语言模拟类
date: 2020-07-20
author: weiwei
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-theme-h2o-postcover.jpg'
tags: C
---
# C语言模拟类

### 1. 使用结构体模拟类属性

```
typedef struct People
{
    char name[10];
    int age;
}People;
```
### 2. 在结构体中使用函数指针来模拟类中的方法
```
typedef struct People
{
    char name[10];
    int age;
    void (*setName)(struct People * self, char * name);
    void (*setAge)(struct People * self, int age);
    char* (*getName)(struct People * self);
    int (*getAge)(struct People * self);
}People;
```
### 3. 定义各种函数来模拟类的实际方法
```
void People_setName(People * self, char * name)
{
    strcpy(self->name, name);
}

void People_setAge(People * self, int age)
{
    self->age = age;
}

char* People_getName(People * self)
{
    return self->name;
}

int People_getAge(People * self)
{
    return self->age;
}

People new_People(char * name, int age)
{
    People people;
    strcpy(people.name, name);
    people.age = age;
    people.setName = People_setName;
    people.setAge = People_setAge;
    people.getName = People_getName;
    people.getAge = People_getAge;

    return people;
}
```
### 4. 总的代码
```
#include<stdio.h>

typedef struct People
{
    char name[10];
    int age;
    void (*setName)(struct People * self, char * name);
    void (*setAge)(struct People * self, int age);
    char* (*getName)(struct People * self);
    int (*getAge)(struct People * self);
}People;

void People_setName(People * self, char * name)
{
    strcpy(self->name, name);
}

void People_setAge(People * self, int age)
{
    self->age = age;
}

char* People_getName(People * self)
{
    return self->name;
}

int People_getAge(People * self)
{
    return self->age;
}

People new_People(char * name, int age)
{
    People people;
    strcpy(people.name, name);
    people.age = age;
    people.setName = People_setName;
    people.setAge = People_setAge;
    people.getName = People_getName;
    people.getAge = People_getAge;

    return people;
}

int main()
{
    People people = new_People("job", 20);
    printf("name:%s age:%d\n", people.getName(&people), people.getAge(&people));
    people.setName(&people, "mike");
    people.setAge(&people, 12);
    printf("name:%s age:%d\n", people.getName(&people), people.getAge(&people));

    return 0;
}
```
### 5. 总结
这样全盘模拟一个类会增加代码量，还会是各种调用变得复杂，可以部分模拟并对其进行简化。