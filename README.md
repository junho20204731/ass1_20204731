# Getopt 및 getopts 명령어
## 1. Getopt 명령어
#### 용도: 
명령행 플래그와 매개변수를 구문 분석합니다.


#### 설명:
getopt 명령은 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석합니다. 플래그는 단일 ASCII 문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 합니다. 인수에는 복수 바이트를 포함시킬 수 있지만 플래그 문자로는 포함시킬 수 없습니다.

getopt 명령은 모든 토큰을 읽은 후 또는 특수 토큰 -(더블 하이픈)이 발생하는 경우 처리를 완료합니다. 그러면 getopt 명령은 처리된 플래그, -(더블 하이픈) 및 남아 있는 토큰을 출력합니다.

토큰이 플래그와 일치하는 데 실패하는 경우 getopt 명령은 메시지를 표준 오류에 기록합니다.

#### 예제:
다음 예제에서와 같이 getopt 명령을 스켈레톤 쉘 스크립트에서 사용하여 옵션을 구문 분석할 수 있습니다.
```
#!/usr/bin/bsh
#parse command line into arguments
set -- `getopt a:bc $*`
#check result of parsing
if [ $? != 0 ]
then
        exit 1
fi
while [ $1 != -- ]
do
        case $1 in
        -a)     # set up the -a flag
                AFLG=1
                AARG=$2
                shift;;
        -b)     # set up the -b flag
                BFLG=1;;
        -c)     # set up the -c flag
                CFLG=1;;
        esac
        shift   # next flag
done
shift   # skip --
# now do the work
.
.
.
```


## 2. Getopts 명령
##### 용도:
명령행 인수를 처리하고 유효한 옵션을 검사합니다.


#### 설명:
getopts 명령은 매개변수 리스트에서 옵션 및 옵션 인수를 검색하는 Korn/POSIX 쉘 내장 명령입니다. 옵션은 +(더하기 부호) 또는 -(빼기 부호)로 시작하고 그 뒤에 문자가 옵니다. + 또는 -로 시작하지 않는 옵션은 OptionString을 종료합니다. getopts 명령은 호출될 때마다 Name의 다음 옵션 값과 쉘 변수 OPTIND에서 처리될 다음 인수의 색인을 배치합니다. 쉘이 호출될 때마다 OPTIND는 1로 초기화됩니다. 옵션이 +로 시작되는 경우 +는 Name의 값 앞에 추가됩니다.

OptionString의 문자 뒤에는 :(콜론)이 오면 옵션에 인수가 있는 것으로 간주됩니다. 옵션에 옵션-인수가 필요한 경우 getopts 명령은 이를 변수 OPTARG에 배치합니다.

OptionString에 포함되지 않은 옵션 문자가 발견되거나 찾은 옵션에 필요한 옵션-인수가 없는 경우:

OptionString이 :(콜론)으로 시작되지 않는 경우
Name은 ?(물음표) 문자로 설정됩니다.
OPTARG가 설정되지 않으며
진단 메시지가 표준 오류에 기록됩니다.
이 조건은 getopts 명령을 처리하는 중에 생긴 오류가 아니라 호출 애플리케이션에 인수가 표시되는 중에 발견된 오류라고 간주됩니다. 진단 메시지는 명시된 대로 기록되지만 종료 상태는 0이 됩니다.

OptionString이 :(콜론)으로 시작되는 경우
Name은 ?(물음표)로 설정되거나 누락된 필수 옵션에 대해서는 :(콜론) 문자로 설정됩니다.
OPTARG는 발견된 옵션 문자로 설정되고
출력이 표준 오류에 기록되지 않습니다.
다음 중 하나가 옵션의 끝을 식별합니다. 특수 옵션 - -,이 - 또는 +로 시작되지 않는 인수를 찾는 경우 또는 오류가 발생하는 경우.

옵션의 끝에 도달하는 경우:

getopts 명령은 0보다 큰 리턴 값으로 종료됩니다.
OPTARG는 첫 번째 비옵션-인수의 색인으로 설정됩니다. 여기서, 첫 번째 - - 인수는 그 전에 다른 비옵션-인수가 나타나지 않는 경우 옵션-인수로 간주되고 비옵션-인수가 없는 경우에는 값 $#+1로 간주됩니다.
Name은 ?(물음표) 문자로 설정됩니다.

#### 예제:

다음 getopts 명령은 a, b 및 c가 유효한 옵션이고 a 및 c 옵션에 인수가 있음을 지정합니다.
```
getopts a:bc: OPT
```

다음 getopts 명령은 a, b 및 c가 유효한 옵션이고 a 및 b 옵션에는 인수가 있으며 명령행에서 정의되지 않은 옵션을 발견하는 경우 getopts가 OPT의 값을 ?로 설정하도록 지정합니다.
```
getopts :a:b:c OPT
```

다음 스크립트가 인수를 구문 분석하고 표시합니다
```

aflag=
bflag=
 
while getopts ab: name
do
            case $name in
            a)     aflag=1;;
            b)     bflag=1
                          bval="$OPTARG";;
            ?)     printf "Usage: %s: [-a] [-b value] args\n" $0
                          exit 2;;
           esac
done
 
if [ ! -z "$aflag" ]; then
           printf "Option -a specified\n"
fi
 
if [ ! -z "$bflag" ]; then
           printf 'Option -b "%s" specified\n' "$bval"
fi
 
shift $(($OPTIND -1))
printf "Remaining arguments are: %s\n" "$*"
```
