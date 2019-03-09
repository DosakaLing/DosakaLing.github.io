---
title: '[ ITDM ] Lesson 2 - R Basic'
date: 2019-03-08 08:20:16
tags:
	- Data Mining
	- R
categories:
	- Programing
	- R
	- Data Mining
---

- Basic usage and example code of R
- The second lesson

<!--more-->

```R
#slide 5
a <- c(1, 2.5, 6, -2)
b <- c("one","two","three")
c <- c(TRUE,TRUE,FALSE,TRUE,FALSE)
x <- vector("numeric", length = 10) #10 zeros in x

#slide 6
a[1]
a[c(1,3,4)]
a[-c(1,3,4)]
# select elements >0
a[c (TRUE,TRUE,TRUE,FALSE)]
a[a>0]

#slide 7
seq(1:10)
seq(1, 10,by=2)
seq(1, 10,length=5)

#slide 8
x<-list(1,"a",TRUE, 1+4i)
x[[2]]

w<-list(name="Fred",mynumbers=c(1,3,5), age=5.3)
w[['age']]

#slide 9
x<-matrix(1:6, nrow=2, ncol=3, byrow=FALSE)
x
dim(x)

#slide 10
x<-matrix(1:6, nrow=2, ncol=3, byrow=TRUE)
x[1,]      #return a vector
x[1,,drop=FALSE] #keep as 1*3 matrix
x[,2]
x[1:2,1:2]
x[x>3]

#slide 11
cells<-1:6
rnames<-c("R1","R2")
cnames<-c("C1","C2","C3")
matrix(cells,nrow=2,ncol=3,byrow=TRUE,dimnames=list(rnames,cnames))

#slide 13
color<-c('red','blue','yellow','red')
is.factor(color)
color<-factor(color)
is.factor(color)

#slide 14
salary<-c('low','medium','high','medium')
factor(salary,ordered=TRUE, levels=c("low","medium","high"))
factor(salary,ordered=TRUE, levels=c("low","high"))  #<NA>:Missing values

#slide 16
ID<-c(1,2,3,4)
Color<-c("black","brown","black","yellow")
Passed<-c(TRUE,TRUE,TRUE,FALSE)
x<-data.frame(ID, Color, Passed)
y<-data.frame(ID, Color, Passed, stringsAsFactors = FALSE)

#slide 17
nrow(x)
ncol(x)
dim(x)
names(x)
x[1:3,]
x$Color
y$Color

#slide 20
data<-read.table("color.txt", header=TRUE, sep="\t")
data<-read.table("color.csv", header=TRUE, sep=",")
data<-read.csv("color.csv", header=TRUE)

#slide 24
grade <-read.csv("data/grade.csv",header=T, stringsAsFactors = FALSE)
student<-read.csv("data/student.csv",header=T, stringsAsFactors = FALSE)

#slide 25
as.factor(c('a','b','a','c'))
as.logical(c(0,1))

#slide 26
student[student$gender=='Female',]

attach(grade)
grade[Chinese>80,]
grade[Chinese>80 && Math>80,]
grade[Chinese>80 | Math>80,]
detach()

#slide 27
grade[order(grade$Chinese),]  #sort by Chinese asc
grade[order(-grade$Chinese),] #sort by Chinese desc

#slide 28

#or use attach
attach(grade)
#sort by chinese asc
grade[order(Chinese),]
#sort by chinese asc, English desc
grade[order(Chinese,-English),]
detach(grade)

#slide 29
total<-merge(student, grade, by="name")
head(total)

#slide 30
attach(total)
#compute the average grade by class
aggregate(total[c("Chinese","Math","English","Science")], by=list(class), FUN=mean)
#compute the average grade by class and gender
aggregate(total[c("Chinese","Math","English","Science")], by=list(class, gender), FUN=mean)
detach(total)

#slide 34
for(i in 1:10) {
  print(i)
}

#slide 35
x <- matrix(1:6, nrow=2, ncol=3, byrow=T)
for(i in 1:nrow(x)){
  for (j in 1:ncol(x)){
    print(paste(i,j,x[i,j]))
  }
}

#slide 36
count <-0
while(count < 10) {
  print(count)
  count <- count + 1
}

#slide 37
x<-1
while(TRUE){	     #infinite loops
  if(x>10){      #terminate condition
    break
  }
  print(x)
  x<-x+1
}
print("outside the while loop")

#slide 40
my_add <- function(a,b){
  return(a+b)
}
my_add(3,5)		

#slide 41
my_add <- function(a, b=0){
  return (a+b)
}
my_add(3)

#slide 42
x1 <- matrix(1:6, nrow=2, ncol=3, byrow=T)
x2 <- matrix(1:6, 2, 3, T)
x3 <- matrix(byrow=T, 1:6, 2, 3)

#slide 43
countPercent <- function(x, ...){ 
  percent <- round(x * 100, ...) 
  paste(percent, "%", sep = "") 
}

countPercent(0.87654)
countPercent(0.87654, digits=2)
```

