https://bankapp1133.streamlit.app/
Click the above link to open application


import streamlit as st

# ---------------- Bank Class ----------------
class BankApplication:
    bank_name = "SBI"

    def __init__(self, name, account_number, age, mobile_number, balance):
        self.name = name
        self.account_number = account_number
        self.age = age
        self.mobile_number = mobile_number
        self.balance = balance

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
            return f"✅ Withdraw Successful: ₹{amount}"
        return "❌ Insufficient Balance"

    def deposit(self, amount):
        self.balance += amount
        return f"✅ Deposit Successful. Balance: ₹{self.balance}"

    def update_mobile(self, new_number):
        self.mobile_number = new_number
        return f"📱 Mobile Updated: {self.mobile_number}"

    def check_balance(self):
        return f"💰 Balance: ₹{self.balance}"


# ---------------- Page Config ----------------
st.set_page_config(page_title="Smart Bank", page_icon="🏦", layout="wide")

# ---------------- CSS Styling ----------------
st.markdown("""
<style>

.stApp {
background: linear-gradient(135deg,#1e3c72,#2a5298);
color:white;
}

.title{
text-align:center;
font-size:50px;
font-weight:bold;
margin-bottom:20px;
}

.subtitle{
text-align:center;
font-size:20px;
margin-bottom:40px;
}

.glass{
background: rgba(255,255,255,0.1);
padding:30px;
border-radius:15px;
backdrop-filter: blur(10px);
box-shadow:0 8px 32px rgba(0,0,0,0.3);
}

.stButton>button{
background: linear-gradient(90deg,#ff9966,#ff5e62);
color:white;
border:none;
border-radius:10px;
height:45px;
font-size:16px;
font-weight:bold;
width:100%;
}

.stButton>button:hover{
transform: scale(1.05);
transition:0.3s;
}

</style>
""", unsafe_allow_html=True)

# ---------------- Title ----------------
st.markdown('<div class="title">🏦 Smart Bank</div>', unsafe_allow_html=True)
st.markdown('<div class="subtitle">Secure • Fast • Modern Banking</div>', unsafe_allow_html=True)

# ---------------- Session Storage ----------------
if "account" not in st.session_state:
    st.session_state.account = None

# ---------------- Sidebar ----------------
menu = st.sidebar.selectbox(
    "Navigation",
    ["Create Account","Deposit","Withdraw","Check Balance","Update Mobile"]
)

# ---------------- Create Account ----------------
if menu == "Create Account":

    st.markdown('<div class="glass">', unsafe_allow_html=True)

    st.subheader("Create Your Bank Account")

    col1,col2 = st.columns(2)

    with col1:
        name = st.text_input("Full Name")
        age = st.number_input("Age",min_value=1)

    with col2:
        account = st.text_input("Account Number")
        mobile = st.text_input("Mobile Number")

    balance = st.number_input("Initial Deposit",min_value=0)

    if st.button("Create Account"):
        st.session_state.account = BankApplication(
            name,account,age,mobile,balance
        )
        st.success("🎉 Account Created Successfully!")

    st.markdown('</div>', unsafe_allow_html=True)

# ---------------- Deposit ----------------
elif menu == "Deposit":

    st.markdown('<div class="glass">', unsafe_allow_html=True)

    st.subheader("Deposit Money")

    if st.session_state.account:

        amount = st.number_input("Amount",min_value=1)

        if st.button("Deposit"):
            result = st.session_state.account.deposit(amount)
            st.success(result)

    else:
        st.warning("Create account first")

    st.markdown('</div>', unsafe_allow_html=True)

# ---------------- Withdraw ----------------
elif menu == "Withdraw":

    st.markdown('<div class="glass">', unsafe_allow_html=True)

    st.subheader("Withdraw Money")

    if st.session_state.account:

        amount = st.number_input("Amount",min_value=1)

        if st.button("Withdraw"):
            result = st.session_state.account.withdraw(amount)
            st.success(result)

    else:
        st.warning("Create account first")

    st.markdown('</div>', unsafe_allow_html=True)

# ---------------- Check Balance ----------------
elif menu == "Check Balance":

    st.markdown('<div class="glass">', unsafe_allow_html=True)

    st.subheader("Account Balance")

    if st.session_state.account:
        st.metric("Current Balance",
                  f"₹{st.session_state.account.balance}")

    else:
        st.warning("Create account first")

    st.markdown('</div>', unsafe_allow_html=True)

# ---------------- Update Mobile ----------------
elif menu == "Update Mobile":

    st.markdown('<div class="glass">', unsafe_allow_html=True)

    st.subheader("Update Mobile Number")

    if st.session_state.account:

        new_mobile = st.text_input("New Mobile Number")

        if st.button("Update"):
            result = st.session_state.account.update_mobile(new_mobile)
            st.success(result)

    else:
        st.warning("Create account first")

    st.markdown('</div>', unsafe_allow_html=True)
