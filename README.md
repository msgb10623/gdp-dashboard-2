import streamlit as st

# 1. 초기 데이터 설정 (제공해주신 리스트)
words = ["apple", "banana", "school", "book", "water"]
meaning = ["사과", "바나나", "학교", "책", "물"]

# 2. 스트림릿 세션 상태(저장소) 초기화
# 웹 페이지가 새로고침되어도 진행 상황과 점수를 기억하기 위함입니다.
if 'index' not in st.session_state:
    st.session_state.index = 0  # 현재 문제 번호 (0부터 시작)
if 'count' not in st.session_state:
    st.session_state.count = 0  # 맞춘 정답 개수

# 앱 화면 타이틀 및 안내
st.title("🇬🇧 영단어 암기 프로그램 🇰🇷")
st.write("제시된 영어 단어를 보고 알맞은 한글 뜻을 입력하세요.")
st.write("---")

# 모든 문제를 아직 다 풀지 않은 경우
if st.session_state.index < len(words):
    # 현재 풀고 있는 단어와 정답 가져오기
    current_word = words[st.session_state.index]
    current_meaning = meaning[st.session_state.index]
    
    # 퀴즈 화면 UI 
    st.subheader(f"문제 {st.session_state.index + 1} / {len(words)}")
    st.info(f"👉 제시된 단어: **{current_word}**")
    
    # 입력값(Input) 위젯 사용: st.text_input
    # 각 문제마다 고유한 위젯 공간을 주기 위해 key를 설정합니다.
    user_input = st.text_input("이 단어의 뜻은 무엇일까요?", key=f"word_input_{st.session_state.index}")
    
    # 버튼 배치를 위한 레이아웃 구성
    col1, col2 = st.columns(2)
    
    with col1:
        # 정답 확인 버튼
        if st.button("정답 확인 ✅"):
            if user_input.strip() == current_meaning:
                st.success("🎉 정답입니다!")
                # 원본 코드의 count(i+1) 로직 반영 -> 맞춘 개수 1 증가
                st.session_state.count += 1
            else:
                st.error(f"❌ 오답입니다. (정답: {current_meaning})")
                
    with col2:
        # 다음 문제 버튼
        if st.button("다음 문제 ➡️"):
            # 다음 단어로 인덱스 증가 후 화면 새로고침
            st.session_state.index += 1
            st.rerun()

# 모든 문제를 다 풀었을 때 나오는 결과 화면 (Output)
else:
    st.balloons()  # 축하 효과 애니메이션
    st.header("🏆 테스트 종료!")
    
    # 최종 결과 출력: st.success 활용
    st.success(f"모든 문제를 다 풀었습니다! 총 {len(words)}문제 중 **{st.session_state.count}**문제를 맞추셨습니다.")
    
    # 다시 풀기 버튼
    if st.button("처음부터 다시 도전하기 🔄"):
        st.session_state.index = 0
        st.session_state.count = 0
        st.rerun()