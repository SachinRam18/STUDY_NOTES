# 3 Ants and Triangle

```text
↗ ↗ ↗  ❌
↙ ↙ ↙  ❌

↗ ↗ ↙  ✅
↗ ↙ ↗  ✅
↙ ↗ ↗  ✅
↙ ↙ ↗  ✅
↙ ↗ ↙  ✅
↗ ↙ ↙  ✅
```

# 10 Coins Puzzle

1. Divide the 10 coins into **2 groups of 5**.

2. Suppose Group 1 has **x heads**.  
   Then Group 2 has **5 - x heads**.

3. Flip all coins in Group 1 → its **x heads become tails** and its **5 - x tails become heads**.  
   So Group 1 now has **5 - x heads**, same as Group 2.

### Example

Group 1: `H H T T T` → **2 heads**  
Group 2: `H H H T T` → **3 heads**

Flip Group 1:

`H H T T T` → `T T H H H` → **3 heads**

Therefore:

**Group 1 = 3 heads**  
**Group 2 = 3 heads** ✅

# Heaven and Hell Puzzle

1. Ask either guard:  
   **"If I ask the other guard which door leads to Heaven, which door will he point to?"**

2. **Both guards will point to the Hell door.**

3. **Choose the opposite door → Heaven.** ✅

# Mislabeled Jars Puzzle

1. Pick **1 item from Jar C** ("Candies & Sweets").  
   Since all labels are wrong, Jar C must contain **only Candies or only Sweets**.

2. Suppose you pick a **Candy** → Jar C = **Candies**.

3. Jar B cannot be Sweets, and Candies is already Jar C → Jar B = **Mixed**.

4. Remaining option → Jar A = **Sweets**.

### Answer

**Minimum items to pick = 1** ✅

**Jar A → Sweets**  
**Jar B → Candies + Sweets**  
**Jar C → Candies**

# Minimum Cut Puzzle

1. **Cut 1:** Cut the 5-unit bar into **1 + 4**.

2. **Cut 2:** Cut the 4-unit piece into **2 + 2**.

3. Final pieces: **1, 2, 2**

### Payment

- **Day 1:** Give `1` → Worker has `1`
- **Day 2:** Give `2`, take back `1` → Worker has `2`
- **Day 3:** Give `1` → Worker has `1 + 2 = 3`
- **Day 4:** Give `2`, take back `1` → Worker has `2 + 2 = 4`
- **Day 5:** Give `1` → Worker has `1 + 2 + 2 = 5`

### Answer

**Minimum cuts = 2** ✅

**Pieces → `1 + 2 + 2`**

