
###✏This algorithm calculates the sum of the elements in the array ###
```java
Algorithm sum(Array,n){
s=0;// ---> execute 1 time
for(i=0;i<n;i++){ // ---> execute n+1 time
s+=A[i];// execute ---> n times
}
return s; //--->execute 1 time
}
```
```
So to calculate the time complexity we say that it will execute:
1+n+1+n+1=2n+3
Hence:
f(n) = 2n+3
and Order of this is O(n)=n (i.e. degree of function is n) 

The sapce complexity of the above will be:
n (size of the array)+1+1+1=n+3

f(n)=n+3
Order of the polynomial = O(n)
### The following algorithm add two nxn matrices
```
```java
Algorithm add(A,B,n){
for(i=0;i<n;i++){//-->execute n+1 (It's own execution times) times **Note: whatever is inside this loop will execute n times**
for(j=0;j<n;j++){//-->execute n(as this is inside the outer loop hence execute n times)x(n+1) times
c[i,j]=A[i,j]+B[i,j];//-->execute nxn times
}
}
}
```
⏲ complexity of the above problem is :
n+1+n^2 +n+n^2=2n^2+2n+1
f(n) = 2n^2 + 2n + 1  
O(n^2)

Space Complexity of the above problem:
A --> n^2
B --> n^2
C --> n^2
n --> 1
i --> 1
j --> 1
Sum = 3n^2 +3 ==> f(n)= 3n^2 +3
O(n^2)
#❓ Analyze the below algorithm
```java
Algorithm 
for(i=1;i<n;i=i*2){
stmt;
}
```
#📓

```
for(i=1;i<n;i=i*2) ---> execute n+1 times
stmt; ---> execute n times as inside the outer loop
}
```

For Time Complexity:

| i     |
| ----- |
| 1     |
| 1*2=2 |
| 2*2=4 |
| 4*2=8 |
| ...   |
| 2^k   |
$2^k=n$
$k=log_2n$
$f(n)=log_2n$
Order of Polynomial is : $O(log_2n)$

Space complexity:
O(n)

#🔗 [AbdulBAri Link to practice](https://www.youtube.com/watch?v=9SgLBjXqwd4&list=PLDN4rrl48XKpZkf03iYFl-O29szjTrs_O&index=7)

# Analysis of if and while loop

#🔗 [Abdul Bari link to while and if analysis](https://www.youtube.com/watch?v=p1EnSvS3urU&list=PLDN4rrl48XKpZkf03iYFl-O29szjTrs_O&index=8)
$\sqrt{n}$-->just for demo


# Important Class of functions
$1<log(n)<\sqrt(n)<n<nlog(n)<n^2<n^3<...<2^n<3^n...<n^n$

