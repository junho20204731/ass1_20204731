# ass1_20204731
ass1_20204731
# getopt 명령어
용도
명령행 플래그와 매개변수를 구문 분석합니다.

설명
getopt 명령은 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석합니다. 플래그는 단일 ASCII 문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 합니다. 인수에는 복수 바이트를 포함시킬 수 있지만 플래그 문자로는 포함시킬 수 없습니다.

getopt 명령은 모든 토큰을 읽은 후 또는 특수 토큰 -(더블 하이픈)이 발생하는 경우 처리를 완료합니다. 그러면 getopt 명령은 처리된 플래그, -(더블 하이픈) 및 남아 있는 토큰을 출력합니다.

토큰이 플래그와 일치하는 데 실패하는 경우 getopt 명령은 메시지를 표준 오류에 기록합니다.

예제
다음 예제에서와 같이 getopt 명령을 스켈레톤 쉘 스크립트에서 사용하여 옵션을 구문 분석할 수 있습니다.

#!/usr/bin/bsh
# parse command line into arguments
set -- `getopt a:bc $*`
# check result of parsing
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
 
