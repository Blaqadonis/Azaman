# Aza Man: AI-Powered Autonomous Financial Assistant

![azaman2.png](azaman2.png)  
___________________________________________________________________________________________
**[Live Aza Man clips]**
![Screen Recording - Mar 15, 2025-VEED.gif](Screen%20Recording%20-%20Mar%2015%2C%202025-VEED.gif)
![Screen Recording - Mar 14, 2025-VEED.gif](Screen%20Recording%20-%20Mar%2014%2C%202025-VEED.gif)

**Aza Man** is my entry for the **Agentic AI Innovation Challenge 2025**. **Aza Man** is a nimble, AI-driven financial assistant crafted to empower users to manage budgets, track expenses, and achieve savings goals effortlessly. It offers a cross-functional experience between CLI and a Streamlit web interface, deployed on Hugging Face Spaces, ensuring seamless state persistence across platforms via SQLite.

**Table of Contents**  

🔹 Vision

🔹 Context and Gaps

🔹 Capabilities

🔹 AI Backbone

🔹 Target Audience

🔹 Design Approach

🔹 Technical Methodology

🔹 Evaluation Strategy

🔹 Dataset Sources & Collection

🔹 Dataset Description

🔹 Dataset Processing Methodology

🔹 Comparative Analysis

🔹 Success/Failure Stories

🔹 Source Credibility

🔹 Deployment Considerations

🔹 Monitoring and Maintenance

🔹 Performance Metrics Analysis

🔹 Key Results Interpretation

🔹 Limitations

🔹 Significance and Implications

🔹 Implementation Insights

🔹 Setup Guide

🔹 Future Roadmap

🔹 Conclusion

### **Vision**
Personal finance shouldn’t feel like a burden. **Aza Man** leverages AI to simplify budgeting, expense tracking, and goal setting, delivering an assistant that cuts through the noise—subscription-free, focused on efficiency, and accessible across multiple interfaces.

### **Context and Gaps**

Tools like Nigeria’s **FairMoney**, **CowryWise**, and **PiggyVest**, alongside global leaders **Mint** and **YNAB**, offer robust features but often demand paid subscriptions or intricate setups. CLI options like **Ledger** are lean but lack AI-driven interactivity. **Aza Man** fills these gaps by:

🔹 Offering free, real-time AI assistance via open-source platforms.
🔹 Supporting both terminal and Streamlit interfaces for diverse user preferences.
🔹 Requiring minimal setup with seamless state persistence across platforms.
🔹 Current solutions often miss conversational depth or predictive analytics—areas **Aza Man** targets, though it currently lacks multi-user support and forecasting.

### **Capabilities**

✅ Budgeting & Goals: Set income and savings targets using the budget tool.

✅ Expense Tracking: Log expenses with categories via the log_expenses tool.

✅ Smart Insights: Deliver precise calculations using the math_tool.

✅ Customized Experience: Retains session and cross-session memory via SQLite, accessible from both terminal ```main.py``` and Streamlit ```app.py```.

✅ Visual Dashboard: Provides expense distribution, savings progress, and trends via Plotly charts on Streamlit.

✅ Cross-Functionality: Use the same login details (user ID) to access state across terminal and web interfaces.

### **AI Backbone**

**Aza Man** harnesses free-tier AI platforms for performance and accessibility:

🔹 Groq: Rapid inference for instant replies.
🔹 Together AI: Open-source LLMs for affordable processing.
🔹 OpenRouter: Flexible model access with tool-calling support.
🔹 Built with LangChain, LangGraph, and LangSmith for graph workflow orchestration, state management, tracing and debugging, it ensures scalability and transparency. 

### **Target Audience**
Ideal for users seeking an AI-powered financial tool—especially people who value simplicity and options.

### **Design Approach**

**Aza Man**’s design emphasizes practicality and accessibility:

🔹 Armed with Tools: Uses financial tools (budget, log_expenses, math_tool) to ensure accurate computations, avoiding LLM hallucination.
🔹 State Persistence: Stores data in SQLite ```memory_agent.db``` for session continuity, accessible via terminal or Streamlit.
🔹 Cost Efficiency: Opts for free-tier platforms (Groq, Together AI, OpenRouter) over premium APIs, trading some model depth for affordability.
🔹 Cross-Platform: Supports terminal ```main.py``` and Streamlit ```app.py```, sharing state via SQLite for a unified experience.
🔹 Visualization: Streamlit dashboard with Plotly for expense and savings insights.

### **Technical Methodology**

**Aza Man**’s workflow ensures seamless operation across platforms:

🔹 Session Initialization: On launch (terminal or Streamlit), Aza Man queries SQLite. If prior data exists for the ```user_id```, it loads it; otherwise, it starts fresh with defaults.
🔹 Input: User enters commands (via terminal or Streamlit chat).
🔹 Processing: The workflow directs input to the LLM, triggering tools as needed. LangSmith logs interactions for analysis.
🔹 State Management: State updates (e.g., username, expenses) are persisted to SQLite.
🔹 Output: Responses are processed and displayed (terminal or Streamlit UI with streaming via ```st_callable_util.py```).
🔹 Session Close: On "exit" or logout, state is saved to SQLite.

### **Evaluation Strategy**
**Aza Man**’s performance is rigorously assessed using ```evals.py``` with [OpenEvals](https://github.com/langchain-ai/openevals):

🔹 Metrics: 
           a. Speed: Response time **< 3 seconds per query (2.92)** on the average, verified via LangSmith tracing.
           b. Accuracy: Uses math_tool for computations, validated by test cases in the evaluation dataset ``` aza_man_eval_results.csv``` .
           c. Usability: Successful state saves per session, confirmed via ```check_db.py```.
🔹 Baseline: Manual spreadsheets.
🔹 Method: ```evals.py``` runs 9 test scenarios from ```aza_man_eval_dataset.csv```,  evaluating outputs against a rubric with a ```ChatTogether``` model as the judge. LangSmith traces execution to debug failures.
🔹 Results: All tests passed as confirmed by ```aza_man_eval_results.csv```.

### **Dataset Sources & Collection**

The only dataset used in **Aza Man** is the evaluation dataset, created for evaluation purposes. It was generated programmatically using a Python script ```create_eval_dataset.py``` to define a sequence of 9 test cases simulating user interactions. These test cases were designed to cover core functionalities like username setup, budget creation, expense logging, and budget tracking, ensuring comprehensive testing of **Aza Man**’s capabilities.

### **Dataset Description**

```aza_man_eval_dataset.csv``` contains 9 rows, each representing a test case with two columns:

🔹 Input: The user’s message.
🔹 Output: The expected assistant response
The dataset is small (9 rows), structured as a CSV file, and focuses on conversational flows in Nigerian Naira (NGN), reflecting a typical user journey for financial management.

### **Dataset Processing Methodology**

Since ```aza_man_eval_dataset.csv``` is a predefined test dataset, minimal processing was required:

🔹 Creation: Generated using a Python script that writes test cases to CSV format, ensuring proper formatting (e.g., financial figures as "X,XXX.00 NGN").
🔹 Cleaning: No cleaning was needed, as the dataset was manually curated for accuracy.
🔹 Transformation: During evaluation, ```evals.py``` reads the CSV, directly mapping inputs to expected outputs for comparison. No handling of missing values or outliers was necessary, as the dataset was specifically designed for **Aza Man**’s evaluation.

### **Comparative Analysis**

Compared to alternatives:

🔹 **[FairMoney](https://fairmoney.io/)**: Offers loans but requires subscriptions; **Aza Man** is free.
🔹 **[PiggyVest](https://www.piggyvest.com/)**: Focuses on savings with a mobile app; **Aza Man** adds conversational budgeting.
🔹 **[Mint](https://mint.intuit.com/)**: Provides forecasting but is subscription-based; **Aza Man** prioritizes simplicity.
🔹 **[YNAB](https://apps.apple.com/ng/app/ynab/id1010865877)**: Detailed budgeting with a learning curve; **Aza Man** is more accessible.
🔹 **[Ledger CLI](https://ledger-cli.org/)**: Terminal-based but lacks AI; **Aza Man** adds conversational AI and web UI..

### **Success/Failure Stories**

🔹 Success: A test user ```blaq01``` can set a 750,000.00 NGN budget, log 182,000.00 NGN in expenses, and confirm remaining funds (268,000.00 NGN) across terminal and Streamlit seamlessly, with Plotly charts visualizing spending patterns as can be confirmed in the videos above showing **Aza Man** live.
🔹 Failure: Early deployment to Spaces failed due to database irregularities, causing state loss on restart. This highlighted the need for cloud persistence, planned for future iterations.
🔹 Lesson: Cross-platform state sharing requires robust persistence; local testing with ```check_db.py``` was crucial for debugging.

### **Source Credibility**

All tools and libraries are cited with versions for reproducibility. See *requirements.txt*.

API providers include [Groq](https://console.groq.com/docs/models), [Together AI](https://api.together.xyz/settings/api-keys), and [OpenRouter](https://openrouter.ai/settings/keys).

### **Deployment Considerations**

**Aza Man** has been successfully deployed to [Spaces](https://huggingface.co/spaces/Blaqadonis/AzaMan), enhancing accessibility:
🔹 Platform: Deployed as a Streamlit app on Spaces.
🔹 Files: Includes:
```
app.py
graph.py
configuration.py
tools.py
state.py
st_callable_util.py
utils.py
prompts.py
azaman2.png
```
🔹 Secrets: API keys (TOGETHER_API_KEY, OPENROUTER_API_KEY, GROQ_API_KEY, LANGCHAIN_API_KEY) are set in Spaces’ Secrets for security. Variables too, like ```PROVIDER``` with value of ```openrouter```, and ```MODEL``` with value set as ```google/gemini-2.0-flash-lite-preview-02-05:free```.
🔹 Persistence: SQLite state (memory_agent.db) resets on deployment; future iterations will explore cloud persistence (e.g., PostgreSQL).

### **Monitoring and Maintenance**

To ensure **Aza Man**’s longevity:

🔹 Metrics: Track response latency, tool call success rate, and SQLite save errors via LangSmith.
🔹 Logging: LangSmith captures LLM interactions, tool calls (e.g., budget, log_expenses), and state updates for debugging.
🔹 Performance: Monitor API rate limits via LangSmith and adjust ```PROVIDER``` in ```.env``` if throttled.
🔹 Maintenance: Regularly update dependencies in *requirements.txt* to address security patches or version conflicts.

### **Performance Metrics Analysis**

1. Response Latency: Averaged **2.92 seconds per query** on Spaces (via LangSmith).

2. Tool Call Success: 100% success rate for budget, log_expenses, and math_tool in test scenarios.

3. State Persistence: SQLite saves succeeded in 100% of sessions, verified by running ```python check_db.py``` after Streamlit interactions.

### **Key Results Interpretation**

🔹 Accuracy: OpenEvals confirmed precise financial calculations
🔹 Usability: Cross-platform state sharing (terminal and Streamlit) worked seamlessly with matching ```user_id```.
🔹 Visualization: Plotly charts on the dashboard provided clear insights into expense distribution and savings progress.

### **Limitations**

**Aza Man** faces constraints:

🔹 Scalability: Single-user SQLite limits concurrent use; Spaces’ stateless nature resets memory_agent.db on restart.
🔹 Insight Depth: Lacks more analytical capabilities due to basic tools and free-tier models.
🔹 Trade-Offs: Free-tier AI may sacrifice some precision compared to paid options.
🔹 Local data storage:Ffuture updates will explore cloud databases.

### **Significance and Implications**

**Aza Man** reimagines financial management with AI-driven simplicity. It offers a free alternative to costly apps, potentially empowering users in regions like Nigeria where mobile banking thrives. Its open-source design and deployment on Spaces could spark innovation in lightweight, conversational finance tools.

### **Implementation Insights**

Practical deployment insights include:

🔹 Challenges: API key setup and understanding Spaces may stump novices.
🔹 Resources: Runs on modest hardware with internet access; Spaces deployment simplifies hosting.
🔹 Scalability: SQLite suffices for prototype but needs upgrades (e.g., PostgreSQL) for scale.
🔹 Best Practices: Use a virtual environment, monitor LangSmith traces, and test state persistence with ```check_state.py```.

### **Setup Guide**

Obtain API Keys: Register at [Groq](https://console.groq.com/docs/models), [OpenRouter](https://openrouter.ai/settings/keys), or [Together AI](https://api.together.xyz/settings/api-keys), and [LangSmith](https://smith.langchain.com/o/f2adffe6-d93b-5c6f-9047-1174f7260035/projects/p/331032bd-528e-437f-acc8-c2170c4430f7?timeModel=%7B%22duration%22%3A%227d%22%7D&peek=b6767fe7-b055-4238-9794-97711e21da2e).

Configure ```.env``` (or Spaces Secrets):
```
GROQ_API_KEY=<your_groq_key>
OPENROUTER_API_KEY=<your_openrouter_key>
TOGETHER_API_KEY=<your_together_key>
PROVIDER=openrouter
MODEL=google/gemmi-2.0-flash-lite-preview-02-05:free
USER_ID=<your_userid>   # (4-10 chars, last 2 digits, e.g. jake00)
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=<your_langsmith_key>
LANGCHAIN_PROJECT=AzaMan-2025
```
Install Dependencies: Run ```pip install -r requirements.txt```.

Launch Locally:
🔹 Terminal: ```python main.py``` for the Command Line Interface.
🔹 Streamlit: ```streamlit run app.py``` for the web interface.

### **Future Roadmap**

🔹 Improved Persistence: Upgrade to cloud databases (e.g., PostgreSQL).
🔹 Advanced Tools: Introduce better forecasting tools and access to updated knowledge maybe an internet tool.
🔹 Enhanced Visuals: Add more interactive charts (e.g., budget forecasts).
🔹 Performance: Optimize LLM calls for faster responses.

### **Conclusion**

**Aza Man** proves AI can simplify financial control with cross-platform functionality, rigorous evaluation, and accessible deployment. It’s fast, efficient, and user-friendly—ideal for streamlined money management. Future iterations will deepen its impact and scalability.

Tags: agents, finance, supervisor

Hub: #chinonsoodiaka

License: MIT 

Spaces: [deployed Aza Man](https://huggingface.co/spaces/Blaqadonis/AzaMan)