---- MY .bashrc file ----
1. Custom Login Dashboard

#nano ~/login-dashboard.sh

--
2. Paste this inside:

#!/bin/bash

# Colors for pretty output
GREEN='\033[0;32m'
NC='\033[0m' # No Color

echo -e "${GREEN}=== Welcome to Your System Dashboard ===${NC}"
echo ""

# Uptime
echo -e "${GREEN}Uptime:${NC}"
uptime
echo ""

# CPU Load
echo -e "${GREEN}CPU Load:${NC}"
top -bn1 | grep "Cpu(s)"
echo ""

# Memory Usage
echo -e "${GREEN}Memory Usage:${NC}"
free -h
echo ""

# Disk Usage
echo -e "${GREEN}Disk Usage:${NC}"
df -h | grep -E '^/dev/'
echo ""

# Network Info
echo -e "${GREEN}IP Address:${NC}"
hostname -I
echo ""

# Running Processes (top 5)
echo -e "${GREEN}Top Processes by CPU:${NC}"
ps -eo pid,comm,%cpu --sort=-%cpu | head -n 10
echo ""
----
3. Then make it executable:

# chmod +x ~/login-dashboard.sh

----
4. Open your bashrc:

# nano ~/.bashrc

At the very end, add:

# Show system dashboard on login (only if terminal is interactive)
if [ -t 1 ]; then
  ~/login-dashboard.sh
fi

----
5. Now save and run:

#source ~/.bashrc

----
