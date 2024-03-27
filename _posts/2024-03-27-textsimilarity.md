---
layout: single
title: 단국대 2023 대기소 기말 프로젝트 - 텍스트 유사도 계산
---

# 코사인 유사도의 정의
코사인 유사도는 두 벡터 사이의 코사인 각도를 이용하여 구하는 두 벡터의 유사도를 의미한다.
두 벡터의 방향이 완전히 동일한 경우에는 1의 값을 가지며, 수직일 경우 0,  수평일  경우 -1의 값을 갖게 된다.
즉, 코사인 유사도는 -1 이상 1 이하의 값을 가지며, 값이 1에 가까울수록 유사도가 높다고 판단할 수 있다.

# 코드의 한계점과 아쉬운 점
1. 정밀한 텍스트 유사도 검사 결과를 기대하긴 어려움, 2개의 텍스트 파일끼리만 검사 가능
2. 프로그래밍 코드끼리의 유사도를 검증할 수 없음
3. 문맥의 무시, 단어의 다의성 고려 불가, 긴 문서 처리의 비효율성

# 소스 코드
~~~python
import re
import math
from collections import Counter

def preprocess(text):
    # 텍스트 전처리 함수: 소문자로 변환, 알파벳 이외의 문자 제거
    text = text.lower()
    text = re.sub(r'[^a-z]', ' ', text)
    return text

def get_cosine_similarity(vec1, vec2):
    # 코사인 유사도 계산
    intersection = set(vec1) & set(vec2)
    numerator = sum([vec1[x] * vec2[x] for x in intersection])

    sum1 = sum([vec1[x] ** 2 for x in vec1])
    sum2 = sum([vec2[x] ** 2 for x in vec2])

    denominator = math.sqrt(sum1) * math.sqrt(sum2)

    if not denominator:
        return 0.0
    else:
        return float(numerator) / denominator

def text_to_vector(text):
    # 단어 빈도를 벡터로 변환, 여러 줄 있어도 사용 가능
    words = text.split()
    return Counter(words)

def main():
    # 텍스트 파일 이름 받아 불러오기
    text1 = input("첫번째 파일 이름: ")
    with open(text1 + '.txt', 'r', encoding='utf-8') as file:
        text1 = file.read()

    text2 = input("두번째 파일 이름: ")
    with open(text2 + '.txt', 'r', encoding='utf-8') as file:
        text2 = file.read()

    # 텍스트 전처리
    text1 = preprocess(text1)
    text2 = preprocess(text2)

    # 텍스트를 벡터로 변환
    vector1 = text_to_vector(text1)
    vector2 = text_to_vector(text2)

    # 코사인 유사도 계산
    similarity = get_cosine_similarity(vector1, vector2)

    print(f"두 텍스트 파일의 유사도: {similarity * 100 : .3f}%")

if __name__ == "__main__":
    main()
~~~

# text1
hello world my name is keum nuri

# text2
hello world my name is lee seunghyun
