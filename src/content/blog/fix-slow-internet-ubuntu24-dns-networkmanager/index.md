---
title: "How to Speed Up DNS Resolution with NetworkManager."
summary: "Experiencing slow internet on Ubuntu 24, especially with websites like GitHub failing to load? While the same network works fine on a MacBook Pro, Ubuntu 24 struggles due to slow DNS resolution. Since systemd-resolved is not available, the best solution is to update DNS settings using NetworkManager and configure Google DNS (8.8.8.8, 8.8.4.4). This guide walks you through identifying the issue, applying the fix, and verifying the improvement, ensuring a faster and more reliable browsing experience on Ubuntu 24"
date: "Feb 25, 2025"
draft: false
tags:   
  - Ubuntu 24
  - Slow Internet
  - NetworkManager
  - DNS Configuration
  - Google DNS
  - Ubuntu Troubleshooting
  - GitHub Loading Issue
  - Fix Internet Speed
  - Linux Networking
  - DNS Resolution
---

# **Fixing Slow Internet on Ubuntu 24: Updating DNS with NetworkManager**  

## **🔍 Problem: Slow Internet on Ubuntu 24, but Works Fine on MacBook Pro**  

Recently, I faced an issue where the **internet speed was slow and unreliable on my HP laptop running Ubuntu 24**, but the same internet connection worked **perfectly fine on my MacBook Pro**.  

### **Symptoms on Ubuntu 24 (HP Laptop)**  
- Websites **took too long to load**, especially **GitHub**, which either **failed to open** or loaded **very slowly**.  
- Other websites sometimes worked, but **performance was inconsistent**.  
- Speed tests showed normal internet speeds, but **DNS resolution was slow**.  

### **MacBook Pro (Same Network, No Issues)**  
On my **MacBook Pro**, everything worked **smoothly with no connectivity issues** on the same Wi-Fi or mobile hotspot.

### **Suspected Issue**  
The issue wasn’t with the internet speed itself but rather with **DNS resolution**—meaning Ubuntu 24 was struggling to resolve website addresses quickly.  

Upon further troubleshooting, I found that **Ubuntu 24 does not use `systemd-resolved` by default**. Instead, **NetworkManager** handles the DNS settings, so fixing it required configuring **Google DNS through NetworkManager**.

---

## **🛠️ Solution: Updating DNS Using NetworkManager**

Since `systemd-resolved` is **not available in Ubuntu 24**, I directly configured **Google DNS (8.8.8.8, 8.8.4.4)** using **NetworkManager**.

### **🔧 Step 1: Identify Active Network Connection**  
First, I checked which network connection was active on my HP laptop (Ubuntu 24):

```bash
nmcli connection show --active
```

**Example Output:**
```
NAME         UUID                                  TYPE      DEVICE
Wi-Fi        12345-abcde-67890-fghij               wifi      wlan0
```
Here, `Wi-Fi` is my **active connection name**.

---

### **🔧 Step 2: Update DNS to Google DNS (8.8.8.8, 8.8.4.4)**  

Next, I updated the DNS settings for my active connection using **NetworkManager**:

```bash
sudo nmcli connection modify "Wi-Fi" ipv4.dns "8.8.8.8 8.8.4.4"
sudo nmcli connection modify "Wi-Fi" ipv4.ignore-auto-dns yes
```

This forces Ubuntu **to bypass the default ISP-provided DNS** and use **Google DNS** instead.

---

### **🔧 Step 3: Restart Network Connection**  
To apply the new DNS settings, I restarted the network:

```bash
sudo nmcli connection down "Wi-Fi" && sudo nmcli connection up "Wi-Fi"
```

---

### **✅ Step 4: Verify the New DNS Settings**  
To ensure the DNS update was successful, I checked the DNS settings again:

```bash
nmcli device show | grep IP4.DNS
```

**Expected Output:**
```
IP4.DNS[1]: 8.8.8.8
IP4.DNS[2]: 8.8.4.4
```

This confirms that Ubuntu is now using **Google's DNS servers**.

---

### **🛠️ Step 5: Testing DNS Resolution & Speed**
After applying the fix, I performed a **DNS lookup test** to verify that Ubuntu was using Google DNS properly:

```bash
dig google.com
```

**Output (Success ✅):**
```
;; SERVER: 8.8.8.8#53(8.8.8.8) (UDP)
```

This confirms that all DNS queries are now being resolved by **Google DNS**, rather than the slow ISP-provided DNS.

---

## **🚀 Results & Improvements**
- ✅ **GitHub and other websites now load instantly** without delays or failures.  
- ✅ **Overall internet performance on Ubuntu 24 significantly improved.**  
- ✅ **MacBook Pro and HP laptop now have similar browsing speeds.**  

---

## **💡 Conclusion**
If you're facing **slow internet on Ubuntu 24**—especially with websites like **GitHub failing to load**—it could be a **DNS resolution issue** rather than a speed problem.  

Since **Ubuntu 24 does not use `systemd-resolved`**, the best way to fix it is to **update DNS settings through NetworkManager** and set it to **Google DNS (8.8.8.8, 8.8.4.4)**.

This method ensures a **faster, more stable, and reliable browsing experience** on Ubuntu.

---

### **🔹 Additional Notes**
- If you are using a **VPN**, make sure it doesn’t override your DNS settings.
- If the issue persists, you can try **disabling IPv6**:
  ```bash
  sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
  ```



---