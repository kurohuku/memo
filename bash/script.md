# Bash シェルスクリプト

## フロー制御
### ループ

```shell
arr=(1 2 3)
for x in ${arr[*]}
do
    echo `expr 1 + $x`
done
# => 2
#    3
#    4

for ((i = 0; i < 3; i++))
do
    echo `expr 1 + $i`
done
# => 1
#    2
#    3

i=0
while [ $i -lt 3 ]
do
    echo $i
    i=`expr 1 + $i`
done
# => 0
#    1
#    2
```


## 関数定義

```shell
function greet() {
    echo Hello, $1
}

greet kurohuku
# => Hello, kurohuku

ret1() {
    return 1
}

ret1
echo $?
# => 1
```

## 変数展開

```shell
a=path/.to/file.txt

## 先頭から最短一致
echo ${a#*/}
# => .to/file.txt

## 先頭から最長一致
echo ${a##*/}
# => file.txt

## 末尾から最短一致
echo ${a%.*}
# => path/.to/file

## 末尾から最長一致
echo ${a%%.*}
# => path/

## 配列の要素数
arr=(1 2 3)
echo ${#arr[*]}
# => 3
echo ${#arr[@]}
# => 3

```
