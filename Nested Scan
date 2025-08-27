#!/bin/bash

read -p "[?] Enter target (domain or IP): " TARGET
TARGET=$(echo "$TARGET" | sed 's~http[s]*://~~')

echo -e "\n[+] Checking target using HTTP/HTTPS"
FINAL_URL=""
if curl -s -I "https://$TARGET" --connect-timeout 5 | head -n 1 | grep -q "200\|301\|302\|403"; then
    FINAL_URL="https://$TARGET"
elif curl -s -I "http://$TARGET" --connect-timeout 5 | head -n 1 | grep -q "200\|301\|302\|403"; then
    FINAL_URL="http://$TARGET"
else
    echo "[!] ERROR: Could not connect to $TARGET on standard web ports (80/443)."
    exit 1
fi
echo "[+] Target is alive: $FINAL_URL"

WORKSPACE="sniper_scan_${TARGET}"

echo -e "\n[+] Starting Recon Stealth Mode"
sniper -t "$TARGET" -m recon -w "$WORKSPACE"
echo "[+] Recon complete. Check loot in: /usr/share/sniper/loot/$WORKSPACE/"

read -p "[?] Continue Surface Scan Selaw Mode? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]
then
    echo -e "\n[+] Starting Selaw Mode Surface Scan"
    sniper -t "$TARGET" -m lazy -w "$WORKSPACE"
fi

read -p "[?] Continue to Deep Scan Full Mode Ngawur? (y/n) " -n 1 -r
echo
if [[ $REPLY =~ ^[Yy]$ ]]
then
    echo -e "\n[!] WARNING: FULL mode is very noisy and aggressive."
    read -p "[?] Are you sure you have permission? (y/n) " -n 1 -r
    echo
    if [[ $REPLY =~ ^[Yy]$ ]]
    then
        echo -e "\n[+] Starting Full Mode"
        sniper -t "$TARGET" -m full -w "$WORKSPACE"
    fi
fi

echo "[+] Sn1per pipeline finished."
echo "[+] All results are in: /usr/share/sniper/loot/$WORKSPACE/"
