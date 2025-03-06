**Step-by-Step Guide: Setting Up a Network Bridge in Windows 7/10 to Connect Two Subnets**

**Objective:**
Allow devices in **Subnet B (192.168.2.0/24)** to access services in **Subnet A (192.168.1.0/24)** using a Windows 7/10 PC as a bridge. The solution ensures minimal IP conflict risks.

---

## **1. Prerequisites**
- A **Windows 7/10 PC** with two network interfaces (NICs):
    - **NIC1** connected to **Subnet A** (192.168.1.0/24)
    - **NIC2** connected to **Subnet B** (192.168.2.0/24)
- Static IP assignments for NIC1 and NIC2.
- Administrator privileges on the Windows PC.
- No DHCP conflicts (DHCP should be enabled on only one network or use static IPs).

---

## **2. Assign Static IP Addresses**
### **For NIC1 (Connected to Subnet A):**
1. Open **Control Panel** → **Network and Sharing Center**.
2. Click **Change adapter settings** (left panel).
3. Right-click **NIC1** → **Properties**.
4. Select **Internet Protocol Version 4 (TCP/IPv4)** → Click **Properties**.
5. Set the following:
      - **IP Address:** `192.168.1.10` (example, ensure no conflict)
      - **Subnet Mask:** `255.255.255.0`
      - **Gateway:** IP of Subnet A's router (e.g., `192.168.1.1`)
      - **DNS:** Use router or external DNS
6. Click **OK** and close the window.

### **For NIC2 (Connected to Subnet B):**
1. Repeat the steps above for **NIC2**.
2. Assign the following:
      - **IP Address:** `192.168.2.10` (example, ensure no conflict)
      - **Subnet Mask:** `255.255.255.0`
      - **Gateway:** Leave blank (to avoid routing conflicts)
      - **DNS:** Leave blank or use the same as NIC1.

---

## **3. Enable Network Bridge in Windows**
1. Go to **Control Panel** → **Network and Sharing Center**.
2. Click **Change adapter settings**.
3. Select **both NIC1 and NIC2** (Ctrl + Click to select both).
4. Right-click and choose **Bridge Connections**.
5. Windows will create a **Network Bridge**, appearing as a new adapter.
6. Verify the bridge:
      - The original adapters (NIC1, NIC2) should now be listed as **Bridged**.
      - The bridge should receive an IP address (if using DHCP) or manually assign an IP.

---

## **4. Configure Client Devices in Subnet B**
For devices in **Subnet B** to access services in **Subnet A**, they need to use the bridge correctly.

### **Option 1: Assign Static IPs on Client Devices**
1. Open **Network Settings** on the client device.
2. Configure a static IP within **Subnet B** (e.g., `192.168.2.100`).
3. Set **Subnet Mask** to `255.255.255.0`.
4. Set **Default Gateway** to the Windows PC's NIC2 IP (`192.168.2.10`).
5. Set **DNS Server** to match Subnet A’s settings (e.g., `192.168.1.1`).

### **Option 2: Use DHCP (If Available)**
- If Subnet B has a **DHCP server**, ensure it assigns a **default gateway of 192.168.2.10**.
- If DHCP is **not available**, manually set the gateway as described in Option 1.

### **Verify Connectivity**
1. Open **Command Prompt** on a client device in **Subnet B**.
2. Run a ping test to a service in **Subnet A**:
   ```
   ping 192.168.1.100
   ```
3. If the response is successful, the bridge is working correctly.
4. Run `tracert` to confirm traffic passes through the bridge:
   ```
   tracert 192.168.1.100
   ```

---

## **5. Prevent IP Conflicts**
### **Option 1: Use Unique Static IPs**
- Ensure no duplicate IPs exist across both subnets.
- Manually assign IPs within each subnet’s range.

### **Option 2: Adjust DHCP Scope**
- If DHCP is enabled, exclude the bridge's IP from DHCP pools.
- Example:
    - Subnet A DHCP range: `192.168.1.50 - 192.168.1.200`
    - Subnet B DHCP range: `192.168.2.50 - 192.168.2.200`

### **Option 3: Use a Different Subnet for Bridged Traffic**
- Instead of bridging `192.168.1.0/24` and `192.168.2.0/24`, use a unique subnet for the bridge (e.g., `192.168.3.0/24`).

---

## **6. Optional: Enable Internet Connection Sharing (If Needed)**
If devices in Subnet B need internet from Subnet A:  

1. Open **Network Connections**.
2. Right-click **Network Bridge** → **Properties**.
3. Go to the **Sharing** tab.
4. Enable **Allow other network users to connect through this computer's Internet connection**.
5. Select the correct network interface providing internet access.

---

## **7. Troubleshooting Tips**
### **Issue: Devices in Subnet B Cannot Access Subnet A**  

✅ Ensure firewall rules are not blocking cross-network traffic.  
✅ Check if the network bridge is properly created and active.  
✅ Run `ipconfig /all` and verify the bridge’s IP configuration.  
✅ Restart the PC after creating the bridge.

### **Issue: IP Conflicts Detected**  

✅ Manually set static IPs outside DHCP ranges.  
✅ Run `arp -a` to check duplicate MAC addresses.  
✅ Avoid assigning a gateway to NIC2.  

### **Issue: No Internet Access from Subnet B**  

✅ Enable **Internet Connection Sharing**.  
✅ Verify the DNS settings.  
✅ Ensure Subnet A’s router allows traffic forwarding.  

---

## **Conclusion**
This guide enables a **Windows 7/10 PC** to bridge two subnets while avoiding IP conflicts. By following these steps, devices in **Subnet B (192.168.2.0/24)** can access services in **Subnet A (192.168.1.0/24)** efficiently.

Let us know if further refinements are needed!

