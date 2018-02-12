# Deploying and configuring KlerosPOC contract

In order to deploy a KlerosPOC arbitrator we need to have two other existing contracts. A random number generator (RNG) and a pinakion (PNK) contract.

### Step 1: Deploy RNG

If you have an existing `RNG` contact deployed you can skip this step and use that same contract

Deploy an instance of a [standardized RNG](https://github.com/kleros/kleros-interaction/blob/master/contracts/standard/rng/RNG.sol)

Example: [BlockhashRNGFallback](https://github.com/kleros/kleros-interaction/blob/master/contracts/standard/rng/BlockhashRNGFallback.sol)

### Step 2: Deploy PNK

There must be a unique `PNK` contract for each instance of `KlerosPOC`

Deploy an instance of [PinakionPOC](https://github.com/kleros/kleros/blob/master/contracts/PinakionPOC.sol)

### Step 3: Deploy KlerosPOC

Deploy an instance of [KlerosPOC](https://github.com/kleros/kleros/blob/master/contracts/KlerosPOC.sol) with the `RNG` and `PNK` contracts you created as well as an int[5] specifying the time for each period in seconds.

Example:
```
KlerosPOC.new(PNKAddress, RNGAddress, [300,300,300,300,300])
```

This would create a new `KlerosPOC` contract where each period lasts 5 mintues.

### Step 4: Configure Contracts

We must configure the `PNK` contract so that it can be used by the instance of `KlerosPOC`

- set the kleros contract
```
PinakionPOCInstance.setKleros(KlerosPOCAddress)
```

- set the owner of the contract to the `KlerosPOC` instance
```
PinakionPOCInstance.transferOwnership(KlerosPOCAddress)
```
