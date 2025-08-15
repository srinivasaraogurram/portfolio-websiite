Amazon DynamoDB offers both a **Free Tier** and a **Pay-as-you-go** pricing model. Here's a breakdown of the costs as of 2024:

---

### ✅ **DynamoDB Free Tier (12 months free for new AWS accounts)**

The AWS Free Tier includes the following DynamoDB usage each month for the first 12 months:

| Resource | Free Tier Allowance |
|--------|---------------------|
| **Read Capacity Units (RCUs)** | 25 million read requests (25 RCU-hours) |
| **Write Capacity Units (WCUs)** | 25 million write requests (25 WCU-hours) |
| **Storage** | 10 GB of storage |
| **Data Transfer** | No additional charge for data transfer in or out within AWS regions (standard AWS data transfer rates apply for cross-region or internet transfers) |

> ✅ This is sufficient for small applications, prototypes, or learning.

---

### 💰 **Pay-as-you-go (On-Demand Mode) – After Free Tier or for Existing Users**

DynamoDB offers two pricing modes: **On-Demand** (pay-per-request) and **Provisioned**. Most users start with **On-Demand** for simplicity.

#### **On-Demand Pricing (Recommended for unpredictable workloads)**

You pay only for what you use:

| Action | Cost |
|-------|------|
| **Read Request** | $0.25 per million read requests (≈ $0.00000025 per read) |
| **Write Request** | $1.25 per million write requests (≈ $0.00000125 per write) |
| **Storage** | $0.25 per GB-month |

> 💡 Example:  
> - 10 million reads = $2.50  
> - 1 million writes = $1.25  
> - 10 GB storage = $2.50/month  
> → Total ≈ **$6.25/month**

---

### 📊 Provisioned Mode (Alternative for Predictable Workloads)

In Provisioned Mode, you set read/write capacity units in advance:

| Resource | Cost (us-east-1) |
|--------|------------------|
| **Read Capacity Unit (RCU)** | $0.00013 per hour (≈ $0.114/month) |
| **Write Capacity Unit (WCU)** | $0.00065 per hour (≈ $0.57/month) |
| **Storage** | $0.25 per GB-month |

> ⚠️ Provisioned mode can be cheaper for steady, predictable traffic but requires capacity planning.

---

### 🔐 Additional Costs (Rarely Apply)
- **Global Tables** – Additional cost for multi-region replication.
- **Backup & Restore** – On-demand backups are free; PITR (Point-in-Time Recovery) costs $0.20 per GB-month.
- **Data Transfer Out to Internet** – Standard AWS data transfer fees apply (e.g., $0.09/GB for first 10TB out to internet).

---

### 💡 Summary: Estimated Monthly Cost

| Usage Level | On-Demand Estimate |
|-----------|--------------------|
| **Low (Free Tier)** | $0 (first 12 months) |
| **Small App (10M reads, 1M writes, 10GB)** | ~$6.25/month |
| **Medium App (100M reads, 10M writes, 50GB)** | ~$18.75/month |
| **High Traffic (1B+ requests)** | Consider Provisioned or auto-scaling |

---

### 🔗 Official Resources
- [DynamoDB Pricing Page](https://aws.amazon.com/dynamodb/pricing/)
- [Free Tier Details](https://aws.amazon.com/free/)

---

✅ **Tip**: Use **On-Demand mode** if you're starting out — it’s simpler and scales automatically. You only pay for what you use, and it’s cost-effective for most small to medium apps.

Let me know your expected traffic, and I can estimate your monthly cost!
