# SnakeShell

> We have employed the best snakes in the industry to perform automatic hacking for you. You simply need to enter the target IP address and number of tries and it will infiltrate the system for you.
>
> All will be done so easily, just like what you see in movies. Experience the power of this tool now!
>
> nc chals.whitehacks.ctf.sg 10111"

The shell has a prompt for number of tries which uses the `input()` function in Python 2. This function does not take your input as a string, and is essentially the same as `eval(raw_input())`.

For example, the payload `7 * 7` would yield `49`.

If we pass the payload `__import__('os').listdir()`, we will realise that there is a peculiar file titled `flag` in the same directory.

By using `__import__('os').open('flag').read()` which reads the `flag` file, we are able to obtain the flag.

`WH2021{Heh_wH0'5_tH3_0ne_wH0_G0T_pwned!}`
