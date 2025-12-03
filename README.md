# IT360-Section1(The Smiths)
# GitHub Repository & Programming Project 
Functionality: This project aims to create a networking tool to solve a forensic science problem. Our tool is a custom “Brute-Force Forensic Evidence Capture & Correlation Tool” which correlates hosts logs to their exact packets, which correlates to moments of compromise!

# Project Goals
1) Detect brute-force login attempts from auth.log
   
2) Slice PCAP to relevant time window  

3) Map timestamps → Wireshark frame number  

4) Provide forensic evidence of SSH compromise


# Code Quality
SSH-Forensic Correlation Tool
Purpose: To map a successful SSH login timestamp to the closest packet in a PCAP capture.
1) Extract accepted login timestamp from auth.log(input command after the letter)


    -A) grep "Accepted password" /var/log/auth.log | tail -n 1 > ~/evidence/accepted.txt


3) Convert timestamp → epoch for correlation(input command after the letter)

    -A) TIMESTR=$(awk '{print $1" "$2" "$3}' ~/evidence/accepted.txt)
   
    -B) YEAR=$(date +%Y)  # Assumes event happened this year
   
    -C) EPOCH=$(date -d "$TIMESTR $YEAR" +%s)

    -D) echo "Converted to epoch: $EPOCH"


4) Compare against packet timestamps extracted earlier using tshark(input command after the letter)

    -A) awk -v e=$EPOCH '
   
    -B) BEGIN {min=1e12; frame=0; time=0}
   
    -C) {
   
    -D) diff = ($2 > e) ? ($2 - e) : (e - $2)
   
    -E) if (diff < min) {min=diff; frame=$1; time=$2}
   
    -F) }
   
    -G) END {print "Closest Frame:", frame, "  Timestamp:", time}
   
    -H) ' ~/evidence/ssh_frames_epoch.txt

#Output should be the closest frame to the intrusion event

# Written Report(will be in the appropriate file as well)
Here lies our pdf report on our project, each step taken to get to the next step, as well as the results that came after the experiment. To give a little insight, were Simulating a cyberattack where the attacker/attackers use a brute force attack  to compromise a password. This is where we come in, we would first monitor network traffic to see when the attack took place, what was possibly changed and then figure out how to record this data to where we could use it to present in court of law. 
([Forensic Project (4).pdf](https://github.com/user-attachments/files/23832079/Forensic.Project.4.pdf))


# Video Presentation
(https://youtu.be/VQ_-nAgAs1A)
