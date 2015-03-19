# Game Of Riches

---

This is a programming challenge based off the game [AdVenture Capitalist](http://hyperhippo.ca/games/adventure-capitalist/).

## Goal Of The Game

---

The goal is to have the most angel investors after 1 year (31,536,000 seconds) of gameplay.

## Angel Investors

---

Angel Investors are the primary way to increase your profit in the long run. Angel Investors can be sacrificed for Angel Upgrades. Angel Investors also increase your profit by `ANGEL_EFFECTIVENESS`% each. For example, 20 angel investors increase your profit by 40%. You earn Angel Investors in proportion to the square root of your life earnings. The catch is that Angel Investors can only be used *after* a reset.

`ANGEL_EFFECTIVENESS` is `2` by default. It can be increased by unlocks and upgrades.

Angel Investors are earned according to this formula: `150 * sqrt(lifeTimeEarnings / 1e15)`

## Money

---

You earn money through the use of businesses. Money is used to purchase businesses and upgrades. The more money you have earned in your lifetime (since your program started execution), the more angels flock to your cause.

## Businesses

---

Businesses can earn you money. There are 10 types of businesses. The price of a business increases exponentially. You can also earn unlocks by achieving certain numbers of businesses. You start with 1 lemonade stand.

    Name               |    Base Price   |  Base Profit / second  | Price Increase per Purchase
    -------------------------------------------------------------------------------------------
    LEMONADE_STAND     |              $4 |                  $1.66 | 7%
    NEWSPAPER_DELIVERY |             $60 |                    $20 | 15%
    CAR_WASH           |            $720 |                    $90 | 14%
    PIZZA_DELIVERY     |          $8,640 |                   $360 | 13%
    DONUT_SHOP         |        $103,680 |                 $2,160 | 12%
    SHRIMP_BOAT        |      $1,244,160 |                 $6,480 | 11%
    HOCKEY_TEAM        |     $14,929,920 |                $19,440 | 10%
    MOVIE_STUDIO       |    $179,159,040 |                $58,320 | 9%
    BANK               |  $2,149,908,480 |               $174,960 | 8%
    OIL_COMPANY        | $25,798,901,760 |               $804,816 | 7%


## Unlocks

---

Unlocks are bonuses that are earned when a set goal has been achieved.

Example list (not actual):

    LEMONADE_STAND;25;LEMONADE_STAND;PROFIT;2
    NEWSPAPER_STAND;900;OIL_COMPANY;PROFIT;11
    ALL;2222;ALL;PROFIT;2

## Upgrades

---

Upgrades are bonuses that are purchased with money or angels. See Angel Upgrades for details on upgrades that are purchased with angels.

Example list (not actual):

    0;CASH;250000;LEMONADE_STAND;PROFIT;3
    1;CASH;500000;NEWSPAPER_DELIVERY;PROFIT;3
    2;CASH;1000000000000000000000;ANGEL;EFFECTIVENESS;1

## Angel Upgrades

---

Angel Upgrades are bonuses that are purchased with the sacrifice of angels. Angels sacrificed for this purpose are **not** regained on reset of a game.

Example list (not actual):

    0;ANGEL;10000;ALL;PROFIT;3
    1;ANGEL;100000000000000000000000000000000;CAR_WASH;COUNT;100
    2;ANGEL;1000000000000000000000000000000000;ANGEL;EFFECTIVENESS;10

## Resetting

---

When you reset your game, you lose all unlocks, upgrades, businesses, and money that you had. You start out with 1 lemonade stand all over again. So why would you want to do that? Because all angels that you may have earned last session are now activated. The angels that you didn't spend last session are also carried over. With these angels, you can earn larger profits faster than those earned in last session.

Lifetime earnings are ***not*** reset when you reset.

# Bot Details

---

Your bot will be an independent program that sends and receives input and output through stdout, and stdin. They are allowed to write to and create files in the directory that they are in. All files that they create must be destroyed on death of the program. The program must be deterministic. If the program does not finish the game within 5 minutes, it is disqualified.

Your bot can send through stdout requests for information. Here is a list of each request along with the reply:

    Request                                                                                    Reply
    ------------------------------------------------------------------------------------------------
    TIME                                                                        game time in seconds
    TIME_LEFT                                                              game time left in seconds
    MONEY                                                                               cash on hand
    ANGELS                                                                             active angels
    ANGELS_SACRIFICED                                       number of angels sacrificed in life-time
    ANGELS_GAIN                                   number of angels that would be gained with a reset
    COUNT type†                                                                       number of type
    COST type†                                                                     cost of next type
    PROFIT type†                                                               profit of all of type
    UPGRADE or CASH_UPGRADE                                     the next cash upgrade you can afford
    ANGEL_UPGRADE                                              the next angel upgrade you can afford
    UPGRADES                                              the newline separated list of all upgrades
    UNPURCHASED_UPGRADES                          the newline separated list of unpurchased upgrades
    PURCHASED_UPGRADES                              the newline separated list of purchased upgrades
    CASH_UPGRADES                                    the newline separated list of all cash upgrades
    UNPURCHASED_CASH_UPGRADES                the newline separated list of unpurchased cash upgrades
    PURCHASED_CASH_UPGRADES                    the newline separated list of purchased cash upgrades
    ANGEL_UPGRADES                                  the newline separated list of all angel upgrades
    UNPURCHASED_ANGEL_UPGRADES              the newline separated list of unpurchased angel upgrades
    PURCHASED_ANGEL_UPGRADES                  the newline separated list of purchased angel upgrades
    NEXT_UNLOCK type†                                                 the next unlock with type type
    UNLOCKS                                                the newline separated list of all unlocks
    UNLOCKED_UNLOCKS                              the newline separated list of all unlocked unlocks
    LOCKED_UNLOCKS                                  the newline separated list of all locked unlocks
    UNLOCKS type†                           the newline separated list of all unlocks with type type
    UNLOCKED_UNLOCKS type†         the newline separated list of all unlocked unlocks with type type
    LOCKED_UNLOCKS type†             the newline separated list of all locked unlocks with type type

<sub>† One of `LEMONADE_STAND`,`NEWSPAPER_DELIVERY`, `CAR_WASH`, `PIZZA_DELIVERY`, `DONUT_SHOP`, `SHRIMP_BOAT`, `MOVIE_STUDIO`, `BANK`, `OIL_COMPANY`, `ALL`</sub>

**These commands return no value:**

    Request                                                                                                  Action
    ---------------------------------------------------------------------------------------------------------------
    WAIT seconds                                                                              warps forward in time
    WAIT_MONEY x                                                                     waits until you have x dollars
    WAIT_ANGEL x                                                                      waits until you have x angels
    BUY type amount                 purchases items one at a time, waiting as needed until you can afford each item
    RESET                                                                                        resets the session
    BUY_CASH_UPGRADE id                  purchases an upgrade, will wait until you can afford the upgrade if needed
    BUY_ANGEL_UPGRADE id          purchases an angel upgrade, will not purchase the upgrade if you cannot afford it

Format of an upgrade string: `id;typeOfUpgrade[1];cost;bonus_string`  
Format of an unlock string: `typeOfUnlock[2];amountNeeded;bonus_string`  
Format of a bonus string: `type[3];subtype[4];amount`

If subtype is `PROFIT` or `COST`, the bonus is applied multiplicatively, otherwise the bonus is applied additively.

<sub>\[1\]: One of `CASH`, `ANGEL`
\[2\]: One of `LEMONADE_STAND`,`NEWSPAPER_DELIVERY`, `CAR_WASH`, `PIZZA_DELIVERY`, `DONUT_SHOP`, `SHRIMP_BOAT`, `MOVIE_STUDIO`, `BANK`, `OIL_COMPANY`, `ALL`  
\[3\]: One of `ANGEL`, `LEMONADE_STAND`,`NEWSPAPER_DELIVERY`, `CAR_WASH`, `PIZZA_DELIVERY`, `DONUT_SHOP`, `SHRIMP_BOAT`, `MOVIE_STUDIO`, `BANK`, `OIL_COMPANY`, `ALL`
\[4\]: `EFFECTIVENESS` if type is `ANGEL`; otherwise one of `COUNT`, `PROFIT`, `COST`</sub>

### Examples:

`0;CASH;1000000;BANK;PROFIT;2` multiplies bank profit by 2 for the cost of 1 million dollars. It has an `id` of `0`.  
`LEMONADE_STAND;100;BANK;COUNT;25` increases the number of banks you own by `25` when you have 100 lemonade stands.

## Controller:

TODO

## Score:

Your score is determined as `log10(ANGEL_TOTAL)`. The person with the largest score wins the contest.

ANGEL_TOTAL is determined by adding all active angels, sacrificed angels, and the number of angels you would gain with a reset.

## Leaderboard:

N/A

tag: `code-challenge`