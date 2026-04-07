# 🚀 Paystream AI

AI-powered payroll system on Arc Testnet using USDC and Circle Developer-Controlled Wallets.

## 📋 Overview

Paystream AI is a full-stack blockchain application that automates payroll processing using:
- **AI Engine**: Google Gemini 2.0 Flash for intelligent processing
- **Wallets**: Circle Developer-Controlled Wallets (SCA on Arc Testnet)
- **Blockchain**: Arc Testnet with USDC
- **Database**: Supabase for employee management
- **Framework**: Next.js 16 with TypeScript

## 🏗️ Architecture

```
CSV Upload → AI Onboarding Agent → Circle Wallets (Arc Testnet)
    → Supabase DB → AI Payroll Agent → Smart Contract Batch Pay → USDC
```

## 📁 Project Structure


```
src/
├── lib/
│   ├── gemini.ts           # Gemini AI wrapper with retry logic
│   ├── ai-agent.ts         # Reusable AI agent class
│   ├── circle.ts           # Circle wallet integration
│   ├── supabase.ts         # Supabase client & operations
│   └── arc.ts              # Arc blockchain & USDC contract
└── app/
    ├── api/
    │   └── health/
    │       └── route.ts    # Health check endpoint
    ├── layout.tsx          # Root layout
    └── page.tsx            # Home page
```

## 🔧 Configuration

All configuration is in `.env.local`:

### Circle (Required)
- `CIRCLE_API_KEY` - Circle API key
- `CIRCLE_ENTITY_ID` - Circle entity secret
- `WALLET_SET_ID` - Circle wallet set ID

### AI (Required)
- `GEMINI_API_KEY` - Google Gemini API key
- `AIMLAPI_KEY` - AI/ML API key
- `AIMLAPI_BASE_URL` - AI/ML API base URL

### Supabase (Required)
- `NEXT_PUBLIC_SUPABASE_URL` - Supabase project URL
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` - Supabase anon key
- `SUPABASE_SERVICE_ROLE_KEY` - Supabase service role key

### Arc Testnet (Required)
- `ARC_RPC_URL` - Arc Testnet RPC URL
- `ARC_CHAIN_ID` - Chain ID (1628)
- `USDC_CONTRACT_ADDRESS` - USDC contract address

### Optional
- `ETHER_PRIVATE_KEY` - Private key for contract deployment


## 🚀 Quick Start

### 1. Install Dependencies
```bash
npm install
```

### 2. Configure Environment
Copy `.env.local` with your credentials (already configured).

### 3. Run Development Server
```bash
npm run dev
```

Server will start at `http://localhost:3000`

### 4. Check System Health
Visit: `http://localhost:3000/api/health`

Expected response:
```json
{
  "status": "ok",
  "gemini": true,
  "circle": true,
  "arc": true,
  "supabase": true
}
```

## 📚 Core Libraries

### 1. Gemini AI (`src/lib/gemini.ts`)
```typescript
import { generate } from '@/lib/gemini';

const response = await generate('Your prompt here');
```

Features:
- Singleton pattern
- Automatic retry (3 attempts)
- Exponential backoff
- Error handling

### 2. AI Agent (`src/lib/ai-agent.ts`)
```typescript
import { AIAgent } from '@/lib/ai-agent';

const agent = new AIAgent('You are a payroll assistant...');
const result = await agent.run('Process this employee data');
```

### 3. Circle Wallets (`src/lib/circle.ts`)
```typescript
import { createEmployeeWallet } from '@/lib/circle';

const { walletId, address } = await createEmployeeWallet('employee@company.com');
```

Features:
- Idempotent wallet creation
- SCA (Smart Contract Account) on Arc Testnet
- Automatic DB check

### 4. Supabase (`src/lib/supabase.ts`)
```typescript
import { insertEmployee, getPendingEmployees } from '@/lib/supabase';

await insertEmployee({
  email: 'employee@company.com',
  name: 'John Doe',
  salary_usd: 5000,
});

const pending = await getPendingEmployees();
```

### 5. Arc Blockchain (`src/lib/arc.ts`)
```typescript
import { getUSDCBalance, transferUSDC } from '@/lib/arc';

const balance = await getUSDCBalance('0x...');
const txHash = await transferUSDC('0x...', '100.50');
```

## 🎯 Next Steps

1. **Create Supabase Table**
   ```sql
   CREATE TABLE employees (
     id SERIAL PRIMARY KEY,
     email VARCHAR(255) UNIQUE NOT NULL,
     name VARCHAR(255) NOT NULL,
     wallet_id VARCHAR(255),
     wallet_address VARCHAR(255),
     salary_usd DECIMAL(10, 2),
     status VARCHAR(50) DEFAULT 'pending',
     created_at TIMESTAMP DEFAULT NOW(),
     updated_at TIMESTAMP DEFAULT NOW()
   );
   ```

2. **Deploy Batch Payer Contract** (coming soon)
   - Smart contract for batch USDC payments
   - Gas optimization
   - Multi-signature support

3. **Build API Endpoints**
   - `/api/onboard` - CSV upload & employee onboarding
   - `/api/payroll/process` - Process payroll run
   - `/api/employees` - Employee management

4. **Add Frontend UI**
   - Dashboard for payroll management
   - Employee list & details
   - Transaction history

## 🔒 Security

- All API keys stored in `.env.local` (never commit to git)
- Service role keys used for server-side operations
- Private key required only for contract deployment
- Circle wallets use SCA (Smart Contract Accounts) for enhanced security

## 🧪 Testing

Test individual components:

```bash
# Test Gemini AI
npx tsx scripts/test-gemini.ts

# Test Circle wallet creation
npx tsx scripts/create-wallet.ts

# Test wallet set creation
npx tsx scripts/create-wallet-set.ts
```

## 📦 Built With

- **Next.js 16** - React framework
- **TypeScript** - Type safety
- **Circle SDK** - Developer-controlled wallets
- **Gemini AI** - Google's AI model
- **Supabase** - PostgreSQL database
- **ethers.js** - Ethereum library
- **Arc Testnet** - Circle's blockchain testnet

## 📄 License

MIT

## 🤝 Support

For issues or questions, please create an issue in the repository.

---

**Current Status:** ✅ All core systems operational
- Gemini AI: Ready
- Circle Wallets: Ready (Wallet Set ID: ${process.env.WALLET_SET_ID})
- Arc Testnet: Ready
- Supabase: Ready
