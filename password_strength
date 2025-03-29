import re
import random
import streamlit as st

# Page Configuration
st.set_page_config(page_title="Password Strength Meter", page_icon="🔐", layout="centered")

# Custom CSS for Improved UI
st.markdown(
    """
    <style>
        body {background-color: #121212; color: white;}
        .stTextInput > div > div > input {
            border-radius: 10px;
            padding: 12px;
            width: 100%;
            background-color: #1E1E1E;
            color: white;
            border: 1px solid #4CAF50;
        }
        .stButton button {
            background-color: #4CAF50;
            color: white;
            font-size: 18px;
            padding: 12px 20px;
            border-radius: 8px;
            width: 100%;
        }
        .stButton button:hover {
            background-color: #45a049;
        }
        .strength-bar {
            height: 12px;
            width: 100%;
            border-radius: 5px;
            margin-top: 10px;
        }
    </style>
    """,
    unsafe_allow_html=True
)

# App Title
st.markdown("""<h1 style='text-align: center;'>🔐 Password Strength Meter</h1>""", unsafe_allow_html=True)

st.write("Enter your password below to check its security level 🔍")

# Function to Generate a Strong Password
def generate_strong_password():
    characters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()_+"
    return ''.join(random.choice(characters) for _ in range(14))

# Function to Estimate Time to Crack Password
def estimate_crack_time(password):
    complexity = 1
    if re.search(r"[A-Z]", password):
        complexity *= 26
    if re.search(r"[a-z]", password):
        complexity *= 26
    if re.search(r"\d", password):
        complexity *= 10
    if re.search(r"[!@#$%^&*()_+]", password):
        complexity *= 10
    combinations = complexity ** len(password)
    seconds_to_crack = combinations / 1e9  # Assuming 1 billion guesses per second
    
    if seconds_to_crack < 60:
        return "🚨 Instantly Crackable!"
    elif seconds_to_crack < 3600:
        return "⚠️ A Few Minutes"
    elif seconds_to_crack < 86400:
        return "⏳ A Few Hours"
    elif seconds_to_crack < 31536000:
        return "🛡️ A Few Months"
    else:
        return "🔒 Several Years"

def check_password_strength(password):
    score = 0
    feedback = []
    
    if len(password) >= 8:
        score += 1
    else:
        feedback.append("⚠️ Password should be at least 8 characters long.")

    if re.search(r"[A-Z]", password) and re.search(r"[a-z]", password):
        score += 1
    else:
        feedback.append("⚠️ Password should include both uppercase and lowercase letters.")

    if re.search(r"\d", password):
        score += 1
    else:
        feedback.append("⚠️ Password should contain at least one number (0-9).")

    if re.search(r"[!@#$%^&*()_+]", password):
        score += 1
    else:
        feedback.append("⚠️ Password should include at least one special character (!@#$%^&*()_+).")
    
    # Strength Bar Colors
    colors = ["#ff4d4d", "#ff944d", "#ffd633", "#33cc33"]
    strength_labels = ["🚫 Weak", "⚠️ Moderate", "✅ Strong", "💪 Very Strong"]
    
    st.markdown(f'<div class="strength-bar" style="background-color:{colors[min(score, len(colors) - 1)]}; width: {score * 25}%"></div>', unsafe_allow_html=True)
    st.subheader(strength_labels[min(score, len(strength_labels) - 1)])
    
    # Time to Crack Estimation
    crack_time = estimate_crack_time(password)
    st.write(f"🕒 Estimated Time to Crack: {crack_time}")
    
    if score == 4:
        st.balloons()
        st.success("🎉 Congrats! You created a super secure password!")
    
    if feedback:
        with st.expander("**🔍 Improve Your Password**"):
            for item in feedback:
                st.write(item)

# Input and Live Feedback
password = st.text_input("Enter your password:", type="password", help="Ensure your password meets the security criteria ✅")

if password:
    check_password_strength(password)  # Live feedback as user types

if st.button("Generate Strong Password"):
    strong_password = generate_strong_password()
    st.text_input("Suggested Strong Password:", value=strong_password, disabled=True)
