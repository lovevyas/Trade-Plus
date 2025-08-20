# 📄 System Design – Trade Plus

## 1. 🎯 Goals
- Provide small businesses with an easy-to-use platform for **sales tracking** and **dynamic pricing**.  
- Keep it **lightweight, cloud-friendly, and scalable**.  
- MVP should work with **manual sales entry**, but architecture should allow future **automation (API integrations/ML)**.  

---

## 2. 👤 User Flow (MVP)
1. **Signup/Login** → seller creates account  
2. **Add products** (name, category, cost price, selling price, stock)  
3. **Upload sales data** (daily/weekly, manual entry or CSV)  
4. **Dashboard shows:**  
   - 📊 Top-selling items  
   - 💤 Slow-moving items  
   - 💡 Suggested price changes (rule-based engine)  
5. Seller decides whether to **update prices**.  

---

## 3. 🏗️ System Architecture (MVP)

```

React Frontend  →  Node.js/Express Backend  →  PostgreSQL (AWS RDS)
↓
Pricing Engine (rule-based)
↓
Notifications (AWS SNS/Email)

```

- **Frontend:** React (dashboard UI)  
- **Backend:** Node.js (REST APIs)  
- **Database:** PostgreSQL (AWS RDS for cloud hosting)  
- **Pricing Engine:** Start with rules → later ML  
- **Notifications:** Email/SMS/Telegram in later phases  

---

## 4. 🗄️ Database Schema (Draft)

### **Users Table**
| Field        | Type        | Description |
|--------------|------------|-------------|
| user_id (PK) | UUID       | Unique ID |
| name         | VARCHAR    | Seller name |
| email        | VARCHAR    | Login email |
| password     | HASHED     | Auth |
| created_at   | TIMESTAMP  | Account creation |

### **Products Table**
| Field          | Type        | Description |
|----------------|------------|-------------|
| product_id (PK)| UUID       | Unique product |
| user_id (FK)   | UUID       | Belongs to seller |
| name           | VARCHAR    | Product name |
| category       | VARCHAR    | Product category |
| cost_price     | DECIMAL    | Buying price |
| selling_price  | DECIMAL    | Current selling price |
| stock          | INT        | Available stock |

### **Sales Table**
| Field        | Type        | Description |
|--------------|------------|-------------|
| sale_id (PK) | UUID       | Unique sale record |
| product_id (FK) | UUID    | Which product |
| date         | DATE       | Sale date |
| quantity     | INT        | Units sold |
| revenue      | DECIMAL    | Quantity * selling price |

---

## 5. ⚙️ APIs (Phase 1 – Core)

### **Auth**
- `POST /auth/signup` → Create account  
- `POST /auth/login` → Login  

### **Products**
- `POST /products` → Add product  
- `GET /products` → List products  
- `PUT /products/:id` → Update product  
- `DELETE /products/:id` → Delete product  

### **Sales**
- `POST /sales` → Add sales record  
- `POST /sales/upload` → Bulk CSV upload  
- `GET /sales/:product_id` → View sales history  

### **Insights**
- `GET /insights/top-products` → Top 5 bestsellers  
- `GET /insights/slow-products` → Low-performing items  
- `GET /insights/price-suggestions` → Suggested prices  

---

## 6. 🔄 Pricing Engine (Rule-Based – MVP)

1. **If demand ↑ (sales > X units in last Y days)** → suggest **+5-10% price**  
2. **If demand ↓ (sales < threshold for Y days)** → suggest **-5% discount**  
3. **If stock low (< Z units)** → send **restock alert**  

➡️ Later, replace this with **ML regression model** for demand forecasting.  

---

## 7. 🚀 Deployment (MVP)

- Backend: **AWS Elastic Beanstalk / Lambda + API Gateway**  
- DB: **AWS RDS (Postgres)**  
- Frontend: **Netlify / Vercel**  
- File storage (CSV uploads): **AWS S3**  

---

## 8. 🛠️ Roadmap (Short-Term)

- **Week 1:** Setup repo + backend skeleton  
- **Week 2:** DB schema + migrations  
- **Week 3:** Auth + Products APIs  
- **Week 4:** Sales APIs + Insights logic  
- **Week 5:** React frontend dashboard (basic UI)  
- **Week 6:** Deploy MVP on AWS  

