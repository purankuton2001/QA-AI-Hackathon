# 🤖 QA AI Hackathon - Multi-Agent Communication System

Demonstration of a distributed AI quality evaluation system using Claude Code

**📖 Read this in other languages:** [日本語](README.md)

## 🎯 Project Overview

This project is a distributed system where multiple Claude Code agents collaborate to evaluate the quality of e-commerce applications. It implements a systematic quality evaluation process through a hierarchical instruction system: PRESIDENT → BOSS → Workers.

### 🏢 System Architecture

```
📊 PRESIDENT Session (Project Management)
└── PRESIDENT: Overall policy decisions and final judgments

📊 multiagent Session (Evaluation Team)
├── boss1: Evaluation team manager
├── worker1: ISTQB compliance & legal regulation evaluation
├── worker2: Management & customer requirements evaluation
└── worker3: Test analyst & technical quality evaluation
```

### 🛒 Target Application for Evaluation

- **Live Demo**: https://ecommerce-with-stripe-six.vercel.app/
- **Repository**: https://github.com/kychan23/ecommerce-with-stripe
- **Technology Stack**: Next.js + Stripe payment integration
- **Market**: E-commerce site for Japanese market (JPY currency support)

## 🚀 Quick Start

### 1. Environment Setup

```bash
# Clone repository
git clone <this-repository>
cd qa-ai-hackathon

# Automatic tmux environment setup
./setup.sh
```

⚠️ **Note**: Existing `multiagent` and `president` tmux sessions will be automatically removed

### 2. Launch Claude Code

```bash
# 1. PRESIDENT authentication (run first)
tmux send-keys -t president 'claude' C-m

# 2. After authentication completion, launch all agents at once
tmux list-panes -t multiagent:agents -F '#{pane_id}' | while # 認証完了後、multiagentセッションを一括起動
for i in {0..3}; do tmux send-keys -t multiagent:0.$i 'claude' C-m; done
```

### 3. System Operation Check

```bash
# Check session status
tmux list-sessions
tmux attach-session -t multiagent  # Check agents
tmux attach-session -t president   # Check management (separate terminal)
```

## 📋 Execution Patterns

### Pattern 1: Hello World Demo (Basic Operation Check)

Execute in PRESIDENT session:
```
You are the president. Follow the instructions.
```

**Expected Operation Flow:**
1. PRESIDENT → boss1: Project start instruction
2. boss1 → workers: Hello World task instruction
3. workers: Task execution + completion file creation
4. Last worker → boss1: All completion report
5. boss1 → PRESIDENT: Completion notification

### Pattern 2: E-commerce Quality Evaluation (Full Operation)

Execute in PRESIDENT session:
```
You are the president. Start e-commerce test evaluation.
```

**Evaluation Process:**
1. **Legal Compliance Evaluation** (worker1)
   - Specific Commercial Transactions Law compliance check
   - PCI DSS & Personal Information Protection Law conformity
   - ISTQB principle-based test perspectives

2. **Business Requirements Evaluation** (worker2)
   - Customer requirements alignment check
   - ROI perspective functional validity assessment
   - Usability & operational efficiency verification

3. **Technical Quality Evaluation** (worker3)
   - Performance test execution
   - Security vulnerability inspection
   - Code quality & maintainability assessment

4. **Integrated Report Creation**
   - Priority ranking of critical issues
   - Release decision (Go/Conditional Go/No-Go)
   - Response time estimation

## 🛠️ Operation Tools

### agent-send.sh - Message Sending

```bash
# Basic usage
./agent-send.sh [agent_name] "[message]"

# Practical examples
./agent-send.sh president "Urgent: Security issue found"
./agent-send.sh boss1 "Request for additional evaluation items"
./agent-send.sh worker1 "Legal requirements re-verification"

# Check agent status
./agent-send.sh --list
```

### Log & Debug Functions

```bash
# Check send history
cat logs/send_log.txt
grep "worker1" logs/send_log.txt

# Check evaluation results
ls -la ./tmp/ecommerce_test_results/
cat ./tmp/ecommerce_test_results/integrated_report.md

# Check work status
ls -la ./tmp/worker*_done.txt
```

## 📁 Project Structure

```
qa-ai-hackathon/
├── README.md                    # This file
├── CLAUDE.md                    # Claude Code configuration
├── setup.sh                     # Environment setup script
├── agent-send.sh                # Message sending tool
├── instructions/                # Agent instruction files
│   ├── president.md             # For supervisor (Hello World)
│   ├── boss.md                  # For team manager (Hello World)
│   ├── worker.md                # For workers (Hello World)
│   ├── president_ecommerce.md   # For supervisor (E-commerce evaluation)
│   ├── boss_ecommerce.md        # For team manager (E-commerce evaluation)
│   ├── worker1_ecommerce.md     # For ISTQB & legal evaluator
│   ├── worker2_ecommerce.md     # For business requirements evaluator
│   └── worker3_ecommerce.md     # For technical quality evaluator
├── ecommerce-with-stripe/       # Target application for evaluation
├── tmp/                         # Work files & evaluation results
└── logs/                        # System logs
```

## 🔧 Troubleshooting

### Environment Reset

```bash
# Complete reset (session deletion + file cleanup)
tmux kill-session -t multiagent 2>/dev/null
tmux kill-session -t president 2>/dev/null
rm -rf ./tmp/*
./setup.sh
```

### Common Issues

1. **tmux session won't start**
   ```bash
   # Check tmux status
   tmux list-sessions
   
   # Force reset
   ./setup.sh
   ```

2. **Claude Code not responding**
   ```bash
   # Re-attach session
   tmux attach-session -t multiagent
   # Reset with Ctrl+C, then re-run
   ```

3. **Evaluation results not generated**
   ```bash
   # Check working directory
   ls -la ./tmp/
   
   # Check logs
   cat logs/send_log.txt
   ```

## 🎯 Deliverables

### Hello World Demo
- `./tmp/worker*_done.txt`: Completion markers for each worker

### E-commerce Quality Evaluation
- `./tmp/ecommerce_test_results/worker1_compliance_report.md`: Legal compliance evaluation
- `./tmp/ecommerce_test_results/worker2_business_report.md`: Business requirements evaluation  
- `./tmp/ecommerce_test_results/worker3_technical_report.md`: Technical quality evaluation
- `./tmp/ecommerce_test_results/integrated_report.md`: Integrated evaluation report

## 🔒 Security Notice

This system should be used **for defensive security purposes only**:
- ✅ Security analysis & vulnerability detection
- ✅ Quality evaluation & compliance verification
- ✅ Defensive tool & detection rule development
- ❌ Malicious code creation & attack technique development

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

## 🤝 Contributing

Contributions via pull requests and issues are welcome!

---

🚀 **Experience Agent Communication!** 🤖✨ 