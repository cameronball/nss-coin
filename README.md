# nsscoin
PoS Blockchain in python cuz I was bored and wanted to create another coin in python.
This is follows on from my other crypto project - 's**tCoin'.

## Made in 3 hours + lots of caffeine

#### Explanation
This coin will use PoS to determine which wallet has won and can forge the next block (incentive: get the fee's, block creation is free...)

##### PoS mechanism
**The wallet with the most score win and can forge the next block**,
in the time period of the block creation every wallet who have more then X score can forge the next block, every node select the one with the highest score.
every block have specific time frame for creation.

```python
    def _calculate_lottery_block_bonus(self, wallet_address: str):
        seed(f"{wallet_address}{self.last_block_hash}")
        lottery_multiplier = choices(LOTTERY_PRIZES, LOTTERY_WEIGHTS)[0]
        return lottery_multiplier

    def _tie_break(self, wallet_address: str) -> float:
        seed(f"{wallet_address}{self.last_block_hash}")
        return random()

    def _calculate_forger_score(self, forger_wallet):
        current_block_index = self.length
        blocks_number = min(
            (current_block_index - forger_wallet["last_transaction"]),
            MAX_BLOCKS_FOR_SCORE,
        )
        lottery_blocks = self._calculate_lottery_block_bonus(forger_wallet["address"])
        blocks_number += lottery_blocks
        multiplier = (blocks_number ** math.e + MIN_SCORE) / (BLOCKS_CURVE_NUMBER ** math.e)
        wallet_balance = min(forger_wallet["balance"], MAX_BALANCE_FOR_SCORE)
        score = wallet_balance * multiplier
        tie_brake_number = self._tie_break(forger_wallet["address"])
        return score+MIN_SCORE + tie_brake_number

```

#### code parts
| Name          | Description                                          |
| ------------- | ---------------------------------------------------- |
| node          | coin p2p network client and server                   |
| blockchain    | manage chain + block foreign + maintaining consensus |

