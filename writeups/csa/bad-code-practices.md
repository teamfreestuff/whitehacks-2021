# Bad Code Practices :​(

> Looking at the codes, it seems that the attacker made multiple commits to hide a secret key.

Previously in `Sweet Tooth`, we found the attacker's GitHub profile ([@chachabooboo](https://github.com/chachabooboo)). If we look through the diffs of all the commits made by him on all his repositories, we notice some suspicious activity.

In one of the commits, there is an odd quote, followed by some text resembling a flag.

![git diff](/images/bad-code-practices-1.png)

In the other commits, there appear to be portions of a flag that can be found by taking the difference.

![git diff](/images/bad-code-practices-2.png)

Our suspicions are confirmed by a comment in one of the commits.

![git diff](/images/bad-code-practices-3.png)

If we record down all of the portions of the flag found, we get the following:
```
j0!n
A
N
ch@113ng1ng
eggc1t!ng
f0r
C5@
fun
car33r
```

To piece these together, we refer to the first diff we found with the odd quote. The quote talks about "a sequence in time", suggesting that we should order the words by commit date. As such, we get:

```
1. eggc1t!ng
2. C5@
3. N
4. fun
5. A
6. ch@113ng1ng
7. car33r
8. f0r
9. j0!n
```

At this point, we need to refer to the weird flag-like text in the `<script>` tag.

`WH2021 9_2_8_5_4_6_3_1_7`

Since the numbers are unique from 1 to 9, we can try using them as indices and place the segments found previously into the respective positions. From there, we can get the flag.

`WH2021{j0!n_C5@_f0r_A_fun_ch@113ng1ng_N_eggc1t!ng_car33r}`
