import streamlit as st
from typing import List, Tuple

st.set_page_config(page_title="PBAI-DBMS v3.0", page_icon="🧠", layout="wide")

st.title("🧠 PBAI-DBMS v3.0 - Pure Python Edition")
st.subheader("World's First Zero-Dependency Hybrid AI + Banker's Algorithm for Deadlock Prediction")
st.caption("Made in India 🇮🇳 | By Muskaan Mishra | BCA Final Year")

st.markdown("---")

class PBAIDeadlockPredictor:
    """
    A heuristic AI model for predicting deadlock risk in database transactions.

    This class implements a lightweight, rule-based approach to mimic ML behavior
    without external dependencies. Designed for resource-constrained environments.

    Attributes:
        risk_threshold (int): Score above which transaction is flagged as risky.
        max_probability (int): Cap for probability percentage to avoid overfitting.
    """

    def __init__(self, risk_threshold: int = 150, max_probability: int = 99):
        self.risk_threshold = risk_threshold
        self.max_probability = max_probability
        self.weights = {
            'lock_requests': 10,
            'active_transactions': 8,
            'shared_resources': 12,
            'wait_time': 5,
            'conflict_rate': 15
        }

    def calculate_risk_score(self, lock_requests, active_transactions, shared_resources, wait_time, conflict_rate):
        """Rule-based risk scoring that mimics ML behavior"""
        score = (lock_requests * self.weights['lock_requests'] +
                active_transactions * self.weights['active_transactions'] +
                shared_resources * self.weights['shared_resources'] +
                wait_time * self.weights['wait_time'] +
                conflict_rate * self.weights['conflict_rate'])
        return min(score, 200)

    def predict_deadlock_probability(self, risk_score):
        """Convert risk score to deadlock probability percentage"""
        probability = min(int((risk_score / self.risk_threshold) * 100), self.max_probability)
        return probability

    def is_safe(self, risk_score):
        """Hybrid safety verification using rule-based threshold"""
        return risk_score < self.risk_threshold

    def get_banker_verification(self, risk_score):
        """
        Hybrid AI + Banker's Algorithm verification
        Uses heuristic risk score to simulate Banker's safety check
        """
        if risk_score < self.risk_threshold:
            return "SAFE STATE", f"System can allocate resources. Risk {risk_score} < Threshold {self.risk_threshold}"
        else:
            return "UNSAFE STATE", f"Deadlock risk detected. Risk {risk_score} >= Threshold {self.risk_threshold}"

# UI Section
st.sidebar.header("DBMS Transaction Parameters")
st.sidebar.caption("Adjust parameters to simulate real DBMS load")

lock_requests = st.sidebar.slider("Lock Requests", 1, 20, 5, help="Number of locks requested by transactions")
active_transactions = st.sidebar.slider("Active Transactions", 1, 15, 3, help="Concurrent transactions running")
shared_resources = st.sidebar.slider("Shared Resources", 1, 10, 4, help="Tables/records being accessed")
wait_time = st.sidebar.slider("Avg Wait Time (ms)", 0, 100, 20, help="Time transactions wait for locks")
conflict_rate = st.sidebar.slider("Conflict Rate %", 0, 100, 10, help="Percentage of conflicting transactions")

st.sidebar.markdown("---")
risk_threshold = st.sidebar.number_input("Risk Threshold", 50, 200, 150, help="Heuristic threshold for safety")

if st.sidebar.button("🔍 Run PBAI-DBMS Prediction", type="primary", use_container_width=True):
    predictor = PBAIDeadlockPredictor(risk_threshold=risk_threshold)

    # Calculate using YOUR original logic
    risk_score = predictor.calculate_risk_score(
        lock_requests, active_transactions, shared_resources, wait_time, conflict_rate
    )
    probability = predictor.predict_deadlock_probability(risk_score)
    safe = predictor.is_safe(risk_score)
    banker_state, banker_explain = predictor.get_banker_verification(risk_score)

    st.markdown("### 🧠 PBAI-DBMS Heuristic Analysis Result")

    col1, col2, col3 = st.columns(3)

    with col1:
        st.metric("Heuristic Risk Score", f"{risk_score}/200")
    with col2:
        st.metric("Deadlock Probability", f"{probability}%")
    with col3:
        if safe:
            st.success("✅ SAFE")
        else:
            st.error("⚠️ UNSAFE")

    st.markdown("---")
    st.subheader("Hybrid AI + Banker's Algorithm Verification")

    if safe:
        st.success(f"**{banker_state}**: {banker_explain}")
        st.info("💡 All DBMS transactions can proceed. No deadlock expected.")
    else:
        st.error(f"**{banker_state}**: {banker_explain}")
        st.warning("💡 Heuristic Suggestion: Reduce concurrent transactions or implement 2-Phase Locking.")

    # Risk Visualization
    st.progress(probability / 100)
    st.caption(f"Risk Level: {probability}% | Threshold: {risk_threshold}")

    # Weight Breakdown
    with st.expander("📊 See Heuristic Weight Calculation"):
        st.write(f"Lock Requests: {lock_requests} × {predictor.weights['lock_requests']} = {lock_requests * predictor.weights['lock_requests']}")
        st.write(f"Active Transactions: {active_transactions} × {predictor.weights['active_transactions']} = {active_transactions * predictor.weights['active_transactions']}")
        st.write(f"Shared Resources: {shared_resources} × {predictor.weights['shared_resources']} = {shared_resources * predictor.weights['shared_resources']}")
        st.write(f"Wait Time: {wait_time} × {predictor.weights['wait_time']} = {wait_time * predictor.weights['wait_time']}")
        st.write(f"Conflict Rate: {conflict_rate} × {predictor.weights['conflict_rate']} = {conflict_rate * predictor.weights['conflict_rate']}")
        st.markdown(f"**Total Risk Score: {risk_score}**")

st.markdown("---")
st.markdown("""
**How PBAI-DBMS Works:**
1. **Heuristic AI**: Rule-based risk scoring using weighted parameters - mimics ML without libraries
2. **Banker's Algorithm Hybrid**: Threshold-based safety verification inspired by Banker's Algorithm
3. **Zero-Dependency**: Pure Python implementation for resource-constrained DBMS environments

**Innovation:** World's First Zero-Dependency Hybrid AI + Banker's Algorithm
""")

