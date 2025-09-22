# Create-and-Configure-VNet-Peering
Hands-on Azure lab demonstrating VNet peering between hub-and-spoke networks, VPN Gateway setup, and connectivity validation via private IPs across VMs.
🌐 Azure Lab: Create and Configure VNet Peering
📖 Overview

In this lab, I created and tested Azure Virtual Network (VNet) peering connections with advanced options such as Gateway Transit and VPN Gateways.
This exercise demonstrated how to securely connect VNets across regions/subscriptions and is foundational for architectures like Hub-and-Spoke.

Skills Demonstrated:

Azure Networking

Virtual Network Peering

VPN Gateway Configuration

Gateway Transit

Network Connectivity Testing (via VM console)
| Item           | Value                                   |
| -------------- | --------------------------------------- |
| Resource Group | `create-and-configure-vnet-peering`     |
| VNets          | `vnet1`, `vnet2`, `vnet3`               |
| Subnets        | Default + GatewaySubnet                 |
| Gateway        | `vpngw` with public IP `vpngwip`        |
| Test VMs       | `vm1`, `vm2`, `vm3`                     |
| Region         | East US (same region for all resources) |

🚀 Step-by-Step Implementation
1️⃣ Configure a VPN Gateway

Open vnet2 → Subnets → +Subnet

Add a GatewaySubnet (required for VPN Gateways).

Go back to the resource group → + Create → Search Virtual Network Gateway.

Configure:

Name: vpngw

Virtual Network: vnet2

Public IP: New → vpngwip

Availability zone: Zone-redundant

Active-active mode: Disabled

Review + Create → Deploy (took ~30 min).

✅ Verified the VPN Gateway deployment completed.

2️⃣ Configure VNet Peering Between vnet1 and vnet2

Go to vnet2 → Peerings → +Add.

Remote link name: vnet1-to-vnet2 → Select vnet1.

Local link name: vnet2-to-vnet1.

Click Add → Connection auto-created both ways.

✅ Verified Peering State = Connected.

3️⃣ Test Connectivity (vnet1 ↔ vnet2)

From vm1 → copied private IP (10.1.1.4).

Reset password for vm2 → logged in via Serial Console.
Ran:hostname -I   # confirmed vm2’s private IP (10.2.2.4)
ping 10.1.1.4

4️⃣ Configure VNet Peering Between vnet2 and vnet3 (Gateway Transit)

Go to vnet2 → Peerings → +Add.

Remote link: vnet3-to-vnet2 → Select vnet3.

Enabled: “Use vnet2’s gateway”.

Local link: vnet2-to-vnet3.

Enabled: “Allow gateway transit”.

Added → Verified Peering = Connected.

5️⃣ Test Connectivity (vnet2 ↔ vnet3)

From vm2 Serial Console:ping 10.3.3.4
✅ Result: 0% packet loss → Gateway Transit + peering successful.
✅ Results

Created VPN Gateway and GatewaySubnet in vnet2.

Successfully configured peering between:

vnet1 ↔ vnet2

vnet2 ↔ vnet3 (with Gateway Transit).

Verified cross-VNet communication using private IPs of VMs.

🧠 Lessons Learned

GatewaySubnet is required for deploying a VPN Gateway.

VNet peering can be bidirectional with just one setup action.

Gateway Transit allows remote VNets to use another VNet’s VPN gateway.

Validating connectivity via private IPs ensures secure communication across VNets.
