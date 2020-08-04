200804 오늘의 소소한 팁



if a>b and a>c and a>d and a>e

이런걸 써야한다면

a > max(b,c,d,e) 이렇게 적자



list comprehension 응용도 가능

res = a - max(b,c,d,e) 

print 'yes' if res > 0 등등 뭐 for 문 넣어도되고