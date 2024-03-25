---
layout: single
title: 2023 단국대 창사코 기말 프로젝트 - 마피아 게임
---

# 특이 규칙
1학기 동안 배운 파이썬의 내용만으로 게임을 구현해야 했고, 마피아 게임의 특성상 비밀 유지가 중요하기에 몇몇 특이한 규칙이 존재한다.

1. 비밀 유지 때문에 플레이어들은 자신의 직업(시민 또는 마피아)을 배정받을 때, 투표할 때 한 명씩 돌아가며 컴퓨터 화면을 봐야 한다.

2. 밤이 되면 모든 플레이어들은 1분간 엎드려야 하며, 마피아는 모든 사람이 엎드린 후 5초 뒤에 조용히 일어나,
   화면을 보고 1분 내에 죽일 사람을 골라야 한다.

3. 초반에 플레이어가 자신의 직업(시민 또는 마피아)을 배정받을 때, 화면에 동그라미 또는 네모가 그려지게 된다.
   이는 화면에 ‘당신은 마피아입니다’ 등의 문구가 출력될 경우 다음 사람이 상대의 직업을 알 수 있기 때문이다.
   따라서 도형을 그렸다가 지우는 화면을 출력하여 직업을 배정하게 되는데, 네모가 출력된 경우 그 사람은 시민이며,
   동그라미가 출력된 경우 그 사람은 마피아가 된다.

# 프로그램의 한계
솔직히 나도 마피아 게임을 구현하며 이렇게까지 복잡해질 줄은 몰랐기에, 여러 부분을 타협하고, 코드의 어떤 부분은 비효율적으로 구현하게 된 부분이 존재한다.

1. 참여 가능인원이 너무 적다는 점 <br/>
   여기서 참여 가능 인원을 더 늘리게 되면 너무 복잡해지기에, 구상 단계에서부터 참여 가능 인원은 정확히 5명으로 제한했다.
   그러나 이렇게 될 경우 게임이 너무 짧게 끝난다는 한계가 존재한다.
   
2. 직업의 개수가 너무 적다는 점 <br/>
   실제로 내가 친구과 마피아 게임을 하게 되면 ‘의사‘ 나 ‘경찰‘ 처럼 다양한 직업을 추가해서 한다.
   그러나 이 게임에 추가하게 되면 사람 수도 적어서 의미가 없어질 뿐만 아니라, 엄청나게 복잡해져 내 능력 밖의 일이 되어버리기에 직업의 개수는 2개로 한정했다.
   
3. if문 남발에 의한 예외상황 처리 불가 <br/>
   이 게임의 대부분은 내가 user_input(‘-> ‘)과 if user_input == ‘1’: 로 연결된다.
   이는 보통의 게임에서 ‘확인하셨습니까?’는 창을 나만의 방식으로 구현한 것인데, 이 과정에서 if에 대한 else를 따로 구현하지 못해 만약 1이나 예정된 값 이외에 다른 값을 입력할 경우 프로그램이 바로 종료된다.
   while 문을 통해 그런 예외상황을 처리해야 한다고 생각하는데, 이 문제점을 발견했을 때에는 이미 코드가 많이 완성된 뒤라, 잘못 건드렸다간 논리 구조가 망가져 처음부터 다시 시작해야 하는 경우가 발생할 수도 있기에 고치지 못했다.
   
4. 비효율적인 코드 <br/>
   이 게임은 참여인원이 5명인 게임이고, 따라서 경우의 수도 비교적 한정적이라 내가 직접 경우의 수를 따져서 코드를 작성할 수 있지만, 이 코드들은 참여인원이 증가할 경우 쓸모가 없어진다.
   예를 들어 뒤에 등장할 투표 알고리즘도 x번째 날 아침, y번째 투표로 죽는 사람, player z 등에서 x, y, z의 관계를 찾아 굳이 저렇게 날마다 코드를 반복하지 않을 수 있는 방법이 존재할 것 같지만 그 부분은 아직 구현하지 못했다.

# 소스 코드
~~~python
mafia_num = 0 # 직업 고를 때 마피아 1명만 나오게 하기 위함

mafia = 0 # 마피아가 누군지 'player(숫자)'로 할당됨

player1 = 0 # 득표수
player2 = 0
player3 = 0
player4 = 0
player5 = 0

playerlist = [player1, player2, player3, player4, player5] # 둘째날 투표 처형당할 사람 찾을 때 사용
playerlist2 = ['player1', 'player2', 'player3', 'player4', 'player5']
playerlist3 = [player1, player2, player3, player4, player5] # 셋째날 투표 처형당할 사람 찾을 때 사용


day1kill = 0 # 마피아가 죽일 사람 번호 입력
day2kill = 0
day3kill = 0

day2vote = 0 # 투표로 죽는 사람
day3vote = 0

cnt = 0 # 2표 득표한 경우(동률로 투표 무효되는 경우)
cnt2 = 0 # 1표가 4개인 경우
cnt3 = 0 # 1표가 3개인 경우

civilnum = 4 # 얘로 경우의 수 나누기

def square():
    for i in range(4):
        t.forward(200)
        t.right(90)
    for j in range(4):
        t.color('white')
        t.forward(200)
        t.right(90)
    t.color('black')
        
def circle():
    t.circle(100)
    t.color('white')
    t.circle(100)
    t.color('black')

print('=' * 21 + '마피아 게임' + '=' * 21)
print('참가 인원은 총 5명이며, 마피아 1명, 시민 4명으로 구성됩니다')
print('게임 시작을 원하시면 숫자 1을 입력하세요')
user_input1 = input('-> ')

if user_input1 == '1':
    print('게임을 시작합니다')
    print('먼저 마피아를 선정합니다')
    print('한 사람씩 차례로 화면을 보되, 다른 사람이 보지 않게 하세요')
    print('화면을 보는 순서대로 게임 내 지시 호칭이 각각 player 1, 2, 3, 4, 5가 됩니다')
    print('화면에 동그라미가 그려지는 사람이 마피아, 네모가 그려지는 사람이 시민입니다')
    print('준비가 되었다면 숫자 1을 입력하세요')
    print('1을 입력하자마자 화면에 동그라미 또는 네모가 그려지니 주의하세요')
    user_input2 = input('-> ')

    if user_input2 == '1':
        job1 = r.randint(1, 5)
        if job1 == 1:
            circle()
            mafia_num = 1
            mafia = 'player1'
        else:
            square()
                        
        print('player1, 당신의 직업을 확인하셨나요?')
        print('확인하셨다면 숫자 1을 입력하세요')
        print('1을 입력하자마자 화면에 동그라미 또는 네모가 그려지니 주의하세요')
        user_input3 = input('-> ')
                
        if user_input3 == '1':
            if mafia_num == 1:
                    square()
            else:
                job2 = r.randint(1, 5)
                if job2 == 1:
                    circle()
                    mafia_num = 1
                    mafia = 'player2'
                else:
                    square()

            print('player2, 당신의 직업을 확인하셨나요?')
            print('확인하셨다면 숫자 1을 입력하세요')
            print('1을 입력하자마자 화면에 동그라미 또는 네모가 그려지니 주의하세요')
            user_input4 = input('-> ')

            if user_input4 == '1':
                if mafia_num == 1:
                    square()
                else:
                    job3 = r.randint(1, 5)
                    if job3 == 1:
                        circle()
                        mafia_num = 1
                        mafia = 'player3'
                    else:
                        square()

                print('player3, 당신의 직업을 확인하셨나요?')
                print('확인하셨다면 숫자 1을 입력하세요')
                print('1을 입력하자마자 화면에 동그라미 또는 네모가 그려지니 주의하세요')
                user_input5 = input('-> ')

                if user_input5 == '1':
                    if mafia_num == 1:
                        square()
                    else:
                        job4 = r.randint(1, 5)
                        if job4 == 1:
                            circle()
                            mafia_num = 1
                            mafia = 'player4'
                        else:
                            square()

                    print('player4, 당신의 직업을 확인하셨나요?')
                    print('확인하셨다면 숫자 1을 입력하세요')
                    print('1을 입력하자마자 화면에 동그라미 또는 네모가 그려지니 주의하세요')
                    user_input6 = input('-> ')

                    if user_input6 == '1':
                        if mafia_num == 1:
                            square()
                        else:
                            circle()
                            mafia = 'player5'

                        print('player5, 당신의 직업을 확인하셨나요?')
                        print('확인하셨다면 숫자 1을 입력하세요')
                        user_input7 = input('-> ')

                        if user_input7 == '1':
                            print('그럼 이제 본격적으로 게임을 시작합니다')
                            print('확인하셨다면 숫자 1을 입력하세요')
                            print('1을 입력하자마자 모든 사람은 5초 이내에 엎드려야 하며')
                            print('마피아는 5초 후에 조용히 일어나 화면을 봐 주세요')
                            print('시민은 밤이 되고 1분 후에 일어나 아침을 맞이하세요')
                            user_input8 = input('-> ')

                            if user_input8 == '1':
                                print('첫째 날 밤입니다')
                                print(str(mafia) + ', 첫번째 날 밤에 죽일 사람을 선택하세요')
                                print('자기 자신을 죽일 수 있으며, 그럴 경우 마피아는 패배하게 됩니다')
                                print('죽일 사람의 번호를 입력하세요')
                                print('예)player 6을 죽이고 싶을 경우 6을 입력하세요')
                                day1kill = input('-> ')

                                print('player' + day1kill, '을 죽입니다')
                                print('확인하셨다면 1을 입력하세요')
                                print('1을 입력하자마자 아침이 되니 주의하세요')
                                user_input9 = input('-> ')

                                if user_input9 == '1':
                                    print('=' * 50)
                                    print('=' * 50)
                                    print('=' * 50)
                                    print('=' * 50)
                                    print('=' * 50)
                                    print('=' * 50)
                                    print('=' * 50)
                                    print('=' * 21 + '보안 유지' + '=' * 21)
                                    print('둘째 날 아침이 되었습니다')

                                    if mafia == 'player' + day1kill:
                                        print('마피아가 자살했으므로 시민팀의 승리입니다')
                                        print('남은 시민 수: {}, 남은 마피아 수: 0'.format(civilnum))
                                        print('게임을 종료합니다')

                                    else:
                                        civilnum = civilnum - 1
                                        print('지난밤 마피아에 의해 player' + day1kill + '이 죽었습니다')
                                        print('남은 시민 수: {}, 남은 마피아 수: 1'.format(civilnum))
                                        print('시민들은 토론을 통해 마피아가 누군지 추리해주세요')
                                        print('3분동안 추리를 마친 뒤 마피아가 누군지에 대한 투표를 진행합니다')
                                        print('추리를 마친 뒤 1을 입력하세요')
                                        user_input10 = input('-> ')

                                        if user_input10 == '1':
                                            print('추리를 마치셨나요?')
                                            print('투표를 진행합니다')
                                            print('한 명씩 차례로 돌아가며 화면을 보세요')
                                            print('화면에 원하는 플레이어의 번호를 입력하세요')
                                            print('예) player6 을 원하는 경우 6을 입력하세요')
                                            print('규칙이 이해되셨다면 1을 입력하세요')
                                            print('1을 입력하자마자 투표가 시작되니 주의하세요')
                                            user_input11 = input('-> ')

                                            if user_input11 == '1':
                                                for a in range(civilnum + 1):
                                                    day2vote = int(input('마피아일 것 같은 사람은?: '))
                                                    playerlist[day2vote - 1] = playerlist[day2vote - 1] + 1
                                                    print('=' * 50)
                                                    print('=' * 50)
                                                    print('=' * 50)
                                                    print('=' * 50)
                                                    print('=' * 50)
                                                    print('=' * 50)
                                                    print('=' * 50)
                                                    print('=' * 21 + '보안 유지' + '=' * 21)
                                                    print('현재 플레이어의 투표가 완료되었습니다')

                                                print('모든 투표가 완료되었습니다')
                                                print('투표의 결과를 확인하려면 1을 입력하세요')
                                                print('1을 입력하자마자 투표 결과가 나타나니 주의하세요')
                                                user_input12 = input('-> ')

                                                if user_input12 == '1':
                                                    for m in playerlist:
                                                        if playerlist[m] == 2:
                                                            cnt = cnt + 1
                                                        elif playerlist[m] == 1:
                                                            cnt2 = cnt2 + 1

                                                    if cnt == 2 or cnt2 == 4:
                                                        print('동률이므로 이번 투표에선 아무도 처형되지 않습니다')
                                                        print('남은 시민 수: {}, 남은 마피아 수: 1'.format(civilnum))
                                                        print('투표결과를 확인했으면 1을 입력하세요')
                                                        user_input13 = input('-> ')

                                                    elif str(day2vote) == mafia:
                                                        print('마피아', str(day2vote) + '가 투표에서 처형되었으므로 시민팀의 승리입니다')
                                                        print('남은 시민 수: {}, 남은 마피아 수 0'.format(civilnum))
                                                        print('게임을 종료합니다')

                                                    else:
                                                        for n in range(5):
                                                            if playerlist[n] == max(playerlist):
                                                                day2vote = playerlist2[n]

                                                        civilnum = civilnum - 1
                                                        print(str(day2vote) + '가 이번 투표에서 처형되었습니다')
                                                        print('남은 시민 수: {}, 남은 마피아 수: 1'.format(civilnum))
                                                        print('투표 결과를 확인했으면 1을 입력하세요')
                                                        user_input13 = input('-> ')

                                                    if user_input13 == '1':
                                                        print('곧 둘째 날 밤이 시작됩니다')
                                                        print('확인하셨다면 숫자 1을 입력하세요')
                                                        print('1을 입력하자마자 모든 사람은 5초 이내에 엎드려야 하며')
                                                        print('마피아는 5초 후에 조용히 일어나 화면을 봐 주세요')
                                                        print('시민은 밤이 되고 1분 후에 일어나 아침을 맞이하세요')
                                                        user_input14 = input('-> ')

                                                        if user_input14 == '1':
                                                            print('둘째 날 밤입니다')
                                                            print(str(mafia) + ', 두번째 날 밤에 죽일 사람을 선택하세요')
                                                            print('자기 자신을 죽일 수 있으며, 그럴 경우 마피아는 패배하게 됩니다')
                                                            print('죽일 사람의 번호를 입력하세요')
                                                            print('예)player 6을 죽이고 싶을 경우 6을 입력하세요')
                                                            day2kill = input('-> ')

                                                            print('player' + day2kill, '을 죽입니다')
                                                            print('확인하셨다면 1을 입력하세요')
                                                            print('1을 입력하자마자 아침이 되니 주의하세요')
                                                            user_input15 = input('-> ')

                                                            if user_input15 == '1':
                                                                print('=' * 50)
                                                                print('=' * 50)
                                                                print('=' * 50)
                                                                print('=' * 50)
                                                                print('=' * 50)
                                                                print('=' * 50)
                                                                print('=' * 50)
                                                                print('=' * 21 + '보안 유지' + '=' * 21)
                                                                print('셋째 날 아침이 되었습니다')

                                                                if mafia == 'player' + day2kill:
                                                                    print('마피아가 자살했으므로 시민팀의 승리입니다')
                                                                    print('남은 시민 수: {}, 남은 마피아 수: 0'.format(civilnum))
                                                                    print('게임을 종료합니다')

                                                                else:
                                                                    civilnum = civilnum - 1
                                                                    print('지난밤 마피아에 의해 player' + day2kill + '이 죽었습니다')
                                                                    print('남은 시민 수: {}, 남은 마피아 수: 1'.format(civilnum))

                                                                    if civilnum == 1:
                                                                        print('마피아의 수와 시민의 수가 동일하므로 마피아의 승리입니다')
                                                                        print('게임이 종료됩니다')

                                                                    else:
                                                                        print('시민들은 토론을 통해 마피아가 누군지 추리해주세요')
                                                                        print('3분동안 추리를 마친 뒤 마피아가 누군지에 대한 투표를 진행합니다')
                                                                        print('추리를 마친 뒤 1을 입력하세요')
                                                                        user_input16 = input('-> ')

                                                                        if user_input16 == '1':
                                                                            print('추리를 마치셨나요?')
                                                                            print('투표를 진행합니다')
                                                                            print('한 명씩 차례로 돌아가며 화면을 보세요')
                                                                            print('화면에 원하는 플레이어의 번호를 입력하세요')
                                                                            print('예) player6 을 원하는 경우 6을 입력하세요')
                                                                            print('규칙이 이해되셨다면 1을 입력하세요')
                                                                            print('1을 입력하자마자 투표가 시작되니 주의하세요')
                                                                            user_input17 = input('-> ')

                                                                            if user_input17 == '1':
                                                                                for a in range(civilnum + 1):
                                                                                    day3vote = int(input('마피아일 것 같은 사람은?: '))
                                                                                    playerlist3[day3vote - 1] = playerlist3[day3vote - 1] + 1
                                                                                    print('=' * 50)
                                                                                    print('=' * 50)
                                                                                    print('=' * 50)
                                                                                    print('=' * 50)
                                                                                    print('=' * 50)
                                                                                    print('=' * 50)
                                                                                    print('=' * 50)
                                                                                    print('=' * 21 + '보안 유지' + '=' * 21)
                                                                                    print('투표가 완료되었습니다')
                                                                                    print('다음 투표를 진행합니다')

                                                                                print('모든 투표가 완료되었습니다')
                                                                                print('투표의 결과를 확인하려면 1을 입력하세요')
                                                                                print('1을 입력하자마자 투표 결과가 나타나니 주의하세요')
                                                                                user_input18 = input('-> ')

                                                                                if user_input18 == '1':
                                                                                    for o in playerlist:
                                                                                        if playerlist[o] == 1:
                                                                                            cnt3 = cnt3 + 1

                                                                                    if cnt3 == 3:
                                                                                        print('동률이므로 이번 투표에선 아무도 처형되지 않습니다')
                                                                                        print('남은 시민 수: {}, 남은 마피아 수: 1'.format(civilnum))
                                                                                        print('투표결과를 확인했으면 1을 입력하세요')
                                                                                        user_input19 = input('-> ')

                                                                                    elif str(day3vote) == mafia:
                                                                                        print('마피아', str(day2vote) + '가 투표에서 처형되었으므로 시민팀의 승리입니다')
                                                                                        print('남은 시민 수: {}, 남은 마피아 수 0'.format(civilnum))
                                                                                        print('게임을 종료합니다')

                                                                                    else:
                                                                                        for p in range(5):
                                                                                            if playerlist[p] == max(playerlist):
                                                                                                day2vote = playerlist2[p]

                                                                                        civilnum = civilnum - 1
                                                                                        print(str(day2vote) + '가 이번 투표에서 처형되었습니다')
                                                                                        print('남은 시민 수: {}, 남은 마피아 수: 1'.format(civilnum))
                                                                                        print('마피아의 수와 시민의 수가 동일하므로 마피아의 승리입니다')
                                                                                        print('게임이 종료됩니다')

                                                                                    if user_input19 == '1':
                                                                                        print('곧 셋째 날 밤이 시작됩니다')
                                                                                        print('확인하셨다면 숫자 1을 입력하세요')
                                                                                        print('1을 입력하자마자 모든 사람은 5초 이내에 엎드려야 하며')
                                                                                        print('마피아는 5초 후에 조용히 일어나 화면을 봐 주세요')
                                                                                        print('시민은 밤이 되고 1분 후에 일어나 아침을 맞이하세요')
                                                                                        user_input20 = input('-> ')

                                                                                        if user_input20 == '1':
                                                                                            print('셋째 날 밤입니다')
                                                                                            print(str(mafia) + ', 세번째 날 밤에 죽일 사람을 선택하세요')
                                                                                            print('자기 자신을 죽일 수 있으며, 그럴 경우 마피아는 패배하게 됩니다')
                                                                                            print('죽일 사람의 번호를 입력하세요')
                                                                                            print('예)player 6을 죽이고 싶을 경우 6을 입력하세요')
                                                                                            day3kill = input('-> ')

                                                                                            print('player' + day3kill,'을 죽입니다')
                                                                                            print('확인하셨다면 1을 입력하세요')
                                                                                            print('1을 입력하자마자 아침이 되니 주의하세요')
                                                                                            user_input21 = input('-> ')

                                                                                            if user_input21 == '1':
                                                                                                print('=' * 50)
                                                                                                print('=' * 50)
                                                                                                print('=' * 50)
                                                                                                print('=' * 50)
                                                                                                print('=' * 50)
                                                                                                print('=' * 50)
                                                                                                print('=' * 50)
                                                                                                print('=' * 21 + '보안 유지' + '=' * 21)
                                                                                                print('넷째 날 아침이 되었습니다')

                                                                                                if mafia == 'player' + day3kill:
                                                                                                    print('마피아가 자살했으므로 시민팀의 승리입니다')
                                                                                                    print('남은 시민 수: {}, 남은 마피아 수: 0'.format(civilnum))
                                                                                                    print('게임을 종료합니다')

                                                                                                else:
                                                                                                    civilnum = civilnum - 1
                                                                                                    print('지난밤 마피아에 의해 player' + day3kill + '이 죽었습니다')
                                                                                                    print('남은 시민 수: {}, 남은 마피아 수: 1'.format(civilnum))

                                                                                                    if civilnum == 1:
                                                                                                        print('마피아의 수와 시민의 수가 동일하므로 마피아의 승리입니다')
                                                                                                        print('게임이 종료됩니다')
print('=' * 5 + '지금까지 마피아 게임을 플레이 해주셔서 감사합니다' + '=' * 5)
~~~
