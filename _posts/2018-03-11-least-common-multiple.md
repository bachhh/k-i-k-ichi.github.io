---
layout: post
title: "Least common multiple up to N"
date: 2018-03-11 12:00:00
categories: algorithms
featured_image: /images/lewis.jpg
---

Problem: Given a non-zero, positive integer N. Output the least common multiple of all the integers in range \[1..N\] ( or maybe output only the last digit)

This is a problem on [ UVA 10680 ] [1], category: number theory, CP3.

## Prime factor

From the theorem in number theory that every positive integer has an unique representation of the form:

$$
n = p_1^{x_1} p_2^{x_2} p_3^{x_3}  ...
$$

Effectively, for some multiple of n aka *kn, could also be represented as

$$
kn = k p_1^{x_1} p_1^{x_1} p_1^{x_1}  ...
$$

Suppose we have 2 numbers n and m, and a third number q which is a least common multiple of both n and m

$$
q = kn = lm

n = p_1^{x_1} p_2^{x_2} p_3^{x_3}  ...
m = p_1^{y_1} p_2^{y_2} p_3^{y_3}  ...
$$

It follows that q can also be represented as

$$
q = p_1^{z_1} p_2^{z_2} p_3^{z_3}  ... \\
z_i = max(x_i, y_i)
$$

Suppose we now have a number Q as the common multiple of all integer in the range of $$[1..k]$$

When adding $$k+1$$ to this interval,
it will at most increase the power of one prime inside Q by 1
( or not at all). Proof of this in the next section

We can see the case where Q will increase is where k+1 is a prime or a power of prime.

The fastest way to solve this problem is to pre-generate an array from
1-10e6 with their respective prime root if they are prime's power, or 0 otherwise
(composite number that are not prime root will never increase Q)


## Code

{% highlight c %}

#include <bits/stdc++.h>

using namespace std;

#define LIMIT 1000000
#define MOD 1000000007ll
#define INF 1e9
typedef long long ll;
typedef pair<ll, ll> ii;
int gcd(ll a, ll b) { return (b==0)?a:gcd(b, a%b); }
int lcm(ll a, ll b) { ll i=(a/gcd(a,b))*b; assert(i>0); return i; }
vector<int> primes;

int last6Digit(ll a){
  while(a%10 == 0) a/=10;
  return a%1000000;
}

int main(){
  //ios::sync_with_stdio(false);
  cin.tie(NULL);

  bitset<LIMIT> bs; bs.set();

  for (int i = 2; i*i < LIMIT; ++i) {
    if(bs[i]){
      for (int j = i*i; j < LIMIT; j+=i) {
        bs[j] = 0;
      }
    }
  }
  for (int i = 2; i < LIMIT; ++i) {
    if(bs[i]) primes.push_back(i);
  }

  static ll pf[1000001]; memset(pf, 0, sizeof(pf[0])*1000000);

  for(int p : primes){
    if(p > 1000000) break;
    ll ind = p;
    while(ind <= 1000000){
      pf[ind] = p;
      ind *= p;
    }
  }

  static ll last[1000001];
  last[1] = 1; last[2] = 2;
  for (int i = 3; i < 1000001; ++i) {
    if(pf[i]){means
      last[i] = last6Digit(pf[i]*last[i-1]);
    }
    else{
      last[i] = last[i-1];
    }
  }

  int n;
  while(cin >> n && n){
    std::cout << last[n]%10 << std::endl;
  }

  return 0;
}

{% endhighlight %}

## Proof

If k+1 is a prime, then adding k+1 to the interval means $$ Q_2 = (k+1) Q_1 $$

Otherwise we assume that adding k+1 increase the power of more than 2 prime number,
or a prime number in Q by more than 2

Which means there exist prime numbers $$ p_1^{x} p_2^{y} $$, which Q
cannot divides them

since K+1 is composite, it can be written as

$$
k+1 = p_1^{x} p_2^{y} Z \\

x \geqslant 1 \ AND \ y \geqslant 1

$$

Here $$ p_1 $$ and $$ p_2 $$ are primes which appears in k+1 but not in Q.
And Z is some other terms.

We can make a new number $$ k_1 = \frac{(k+1){ p_2^{y} } = p_1^{x} Z $$

Now $$ k_1 $$ is sure to be less than or equal to $$ k $$, and since
$$ Q $$ is the LCM of all integer from 1 to $$ k $$, it must be divisible
by $$ p_1 $$, this contradict with the assumption that Q cannot divides both
$$ p_1 $$ and $$ p_2 $$

### References:

[1]: https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1621
