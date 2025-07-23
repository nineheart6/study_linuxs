
## 인자와 read를 함께 사용하여 변수 조합 출력

```bash
#출력결과
~$ 80_2_shell_variables_read.sh agument_first
 read input : read_first

input values : agument_first read_first
```

### 풀이
```bash
[seungjae@localhost Downloads]$ nano shell_read.sh

V_ARGUMENT="$1"
read -p "read input: " V_READINPUT 
echo "input value: $V_ARGUMENT $V_READINPUT"

[seungjae@localhost Downloads]$ source shell_read.sh argument_first
read input: read_first
input value: argument_first read_first

```