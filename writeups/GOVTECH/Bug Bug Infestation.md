# Bug Bug Infestation

> Our employee has been complaining of strange PDF files been downloaded onto his laptop. As a reactive measure, our forensics team have setup network monitoring to capture any strange network behaviour coming from his laptop.
>
> Investigate this pcap file and find evidence of a possible BugBug Malware Infection?

If we look through the pcap file, it seems like there are some files being transferred over `HTTP`. We can filter them out using `File > Export Objects > HTTP`.

![HTTP object list](/images/Bug%20Bug%20Infestation_1.png)

When we look at the packets in `PDF_1.pdf`, we notice something peculiar at the end of the file...

![packets](/images/Bug%20Bug%20Infestation_2.png)

At the end of every `PDF` file, there appears to be some fragments of a `PNG` file in between `%BUG` delimiters. When we reconstruct the `PNG` file, we are able to get an image of the flag.

`WH2021{B()GBuGL1veSOn}`
