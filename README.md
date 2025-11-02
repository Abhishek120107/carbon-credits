# Carbon Credits — Mini Hackathon

#**Problem statement**

The current carbon credit system is centralised, opaque, and inefficient, making it 
difficult for industries and governments to verify, trade, and track genuine carbon credits. 
This leads to fraud, double-counting, and low trust in the sustainability market.
## Intro
## What this repo contains
- Short explainers (beginner → technical)
- Code: credit calculator, registry browser demo, visualisation notebook


## 1) What is a carbon credit? (simple)
A **carbon credit** is a tradable certificate that represents **one metric tonne of CO₂ equivalent (1 tCO₂e)** that has been reduced, avoided, or permanently removed from the atmosphere. Projects that create credits (for example, a forest protection project or a methane capture system) are verified by recognised standards and then issued credits which can be sold. 
When a credit is used to compensate for emissions, it is **retired** so it cannot be sold again.
For example:
- Planting trees → earns credits (removes CO₂)
- Burning fossil fuels → spends credits (adds CO₂)
## 2) Why carbon credits are used (short)
- **Channel finance:** They direct private money to projects (renewables, reforestation, methane capture) that cut or remove greenhouse gases.
- **Corporate emissions strategy:** Companies use credits to compensate for emissions they cannot yet eliminate while reducing their operational emissions first.  
- **Support sustainable development:** Some standards require projects to deliver social or environmental co-benefits (jobs, health, biodiversity).

## 3) Important quality concepts (very short)
- **Additionality:** A project must produce reductions that **would not have happened without the carbon-credit revenue**. This is central to a credit’s credibility. 
- **Permanence & leakage:** For removals (like trees), permanence asks whether carbon will stay stored; leakage asks whether emissions were simply displaced elsewhere.  
- **Verification, registries & blockchain:**  
  Standards (e.g., Verra, Gold Standard) and registries record the creation and retirement of carbon credits so each credit is traceable and cannot be double-counted.  
  However, many traditional registries are centralised and not easily interoperable.  
  **By using blockchain technology — such as the Stellar network — the issuance, transfer, and retirement of carbon credits can be stored on a transparent, tamper-resistant ledger.**  
  This ensures that data cannot be secretly changed, all transactions are publicly verifiable, and credits can be represented as digital tokens that move quickly and securely between participants.


## 4) Caveats & the market today (short)
-The voluntary carbon market has real value but also real problems: some credits have been found to fail modern integrity tests, and reforms are underway to improve quality and trust. Always prefer credits with clear verification, recent monitoring data, and co-benefits.


## 5) Brief overview of Roadmap
Frontend (React.js)  →
Backend API (Node.js / Flask) →
Stellar Blockchain (Soroban Smart Contracts)→
IPFS (for certificate storage).

---


## Simple code in Rust
// src/main.rs
use std::env;
use std::process;

fn credits_needed(emissions_tco2: f64) -> u64 {
    emissions_tco2.ceil() as u64
}

fn print_usage_and_exit(program: &str) -> ! {
    eprintln!("Usage: {} <emissions in tCO2e>", program);
    eprintln!("Example: {} 8.8", program);
    process::exit(1);
}

fn main() {
    let args: Vec<String> = env::args().collect();
    let prog = &args[0];

    if args.len() < 2 {
        print_usage_and_exit(prog);
    }

    let input = &args[1];
    let emissions: f64 = match input.parse() {
        Ok(v) => v,
        Err(_) => {
            eprintln!("Error: could not parse '{}' as a number.", input);
            print_usage_and_exit(prog);
        }
    };

    if emissions < 0.0 {
        eprintln!("Error: emissions cannot be negative.");
        process::exit(1);
    }

    let credits = credits_needed(emissions);
    println!("Emissions: {:.2} tCO₂e", emissions);
    println!("Credits needed (1 credit = 1 tCO₂e): {}", credits);
}


