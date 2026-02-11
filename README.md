import streamlit as st
from datetime import datetime

# --- 1. 頁面外觀美化 (CSS) ---
st.set_page_config(page_title="DS-Squad 街頭開發系統", layout="centered")

st.markdown("""
    <style>
    /* 讓按鈕變大變圓潤 */
    .stButton>button { width: 100%; border-radius: 12px; height: 3.5em; background-color: #2E5BFF; color: white; border: none; font-weight: bold; }
    /* 卡片式設計 */
    .metric-card { background-color: #ffffff; padding: 20px; border-radius: 15px; border: 1px solid #f0f0f0; text-align: center; box-shadow: 0 4px 6px rgba(0,0,0,0.05); }
    /* 底部導覽列模擬 */
    .stTabs [data-baseweb="tab-list"] { gap: 10px; justify-content: space-around; }
    </style>
    """, unsafe_allow_html=True)

# --- 2. 登入與流程控管 (Session State) ---
if 'stage' not in st.session_state:
    st.session_state.stage = 'login'

# --- 階段 A：登入頁面 ---
if st.session_state.stage == 'login':
    st.title("DS-Squad 街頭開發")
    st.caption("請登入以開始使用")
    
    phone_acc = st.text_input("電話號碼 (帳號)", placeholder="09xx123456")
    password = st.text_input("密碼", type="password", placeholder="****")
    
    if st.button("立即登入"):
        if phone_acc and password: # 這裡暫不設限密碼，有打字就能進
            st.session_state.stage = 'select_partner'
            st.rerun()
        else:
            st.error("請輸入帳號密碼")

# --- 階段 B：選擇夥伴 ---
elif st.session_state.stage == 'select_partner':
    st.header("👤 出勤與動態組隊")
    st.info("嗨，夥伴！請選擇今日一起作業的夥伴：")
    
    partners = ["劉耘均 (引薦人:巴其哥)", "陳啟祥 (引薦人:巴其)", "連修弘 (引薦人:蚊子)", "唐筠涵 (引薦人:嘎嘎)"]
    selected = st.radio("夥伴名單：", partners)
    
    if st.button(f"與 {selected.split(' ')[0]} 開始作業"):
        st.session_state.partner = selected
        st.session_state.stage = 'main_app'
        st.rerun()

# --- 階段 C：進入四大分頁系統 ---
elif st.session_state.stage == 'main_app':
    
    # 底部導覽列
    tab1, tab2, tab3, tab4 = st.tabs(["📈 概況", "📋 問卷", "🕒 紀錄", "🏆 排行"])

    # --- Tab 1: 概況 ---
    with tab1:
        st.subheader("DS-Squad 概況")
        st.write(f"當前小隊：**我 & {st.session_state.partner.split(' ')[0]}**")
        
        c1, c2 = st.columns(2)
        with c1:
            st.markdown('<div class="metric-card"><p>今日名單數</p><h2 style="color:#2E5BFF">0</h2></div>', unsafe_allow_html=True)
        with c2:
            st.markdown('<div class="metric-card"><p>目前累積經驗</p><h2 style="color:#FF8C00">0 EXP</h2></div>', unsafe_allow_html=True)
        
        st.warning("**重要提醒**\n1. 問卷請務必確認電話真實性。\n2. 滿 90 天未成交名單將釋出。")
        if st.button("🏁 結束今日作業 (結算)"):
            st.session_state.stage = 'login'
            st.rerun()

    # --- Tab 2: 問卷 (照你提供的題目修改) ---
    with tab2:
        st.subheader("問卷表單")
        
        st.write("**1. 您對哪個項目有興趣嗎？(可複選)**")
        interests = [st.checkbox(i) for i in ["跳床", "美容", "越式洗頭", "AI手搖飲"]]
        
        st.write("**2. 您滿意自己的身型嗎？**")
        q2 = st.radio("滿意度", ["滿意", "尚可", "不滿意"], horizontal=True, label_visibility="collapsed")
        
        st.write("**3. 您有什麼樣的需求嗎？(可複選)**")
        needs = [st.checkbox(n) for n in ["我想減重", "我想雕塑", "我想增重"]]
        
        st.write("**4. 您試過什麼方式調整體態？(可複選)**")
        m_list = ["少吃多動", "減肥藥", "中醫調理", "保健食品", "健身房", "醫美診所", "其他"]
        methods = [st.checkbox(m) for m in m_list]
        
        st.write("**5. 改變體態的決心 (1-10分)**")
        score = st.select_slider("決心", options=list(range(1, 11)), value=5)
        
        st.divider()
        name = st.text_input("姓名 *")
        age = st.text_input("年齡")
        phone = st.text_input("電話 *")
        
        if st.button("📤 送出資料"):
            if name and phone:
                st.balloons()
                st.success("感謝您幫我們做問卷！已獲得機票抽獎機會！")
                # 這裡可以放之前教你的複製文字區塊
            else:
                st.error("姓名跟電話是必填喔！")

    # --- Tab 3: 紀錄 ---
    with tab3:
        st.subheader("我的戰果 (近10日)")
        st.info("近 10 天尚無開發紀錄，加油！去填寫第一份問卷吧！")

    # --- Tab 4: 排行 ---
    with tab4:
        st.subheader("排行榜")
        st.write("🏆 目前總榜排名：")
        st.table([{"排名": 1, "姓名": "李佳鴻", "EXP": 1300}, {"排名": 2, "姓名": "蔡尊守", "EXP": 1200}])# dsds
