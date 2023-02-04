# Hash cracking using hashcat

I did some testing to crack NTLM hash without any prior knowledge of password policy I used all small case,uppercase and special characters to crack hash of "password"  which has 8 char as we don't know the password length we can try to bruteforce it by trying all combination of 8 char password then 9,10,12 so on.. when I tried it hashcat showed that it will take 1 day 6 hours to try all combination but cracked it in about 3 sec with speed of 4800 MH/s on my hardware.

generate NTLM hash: https://codebeautify.org/ntlm-hash-generator

```
> cat hash
8846F7EAEE8FB117AD06BDD830B7586C
> hashcat -a 3 -m 1000 --increment --increment-min 8 --increment-max 12 q ?a?a?a?a?a?a?a?a?a?a?a?a?a -D 1,2

8846f7eaee8fb117ad06bdd830b7586c:password                 
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1000 (NTLM)
Hash.Target......: 8846f7eaee8fb117ad06bdd830b7586c
Time.Started.....: Sat Feb  4 13:37:04 2023 (1 min, 27 secs)
Time.Estimated...: Sat Feb  4 13:38:31 2023 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Mask.......: ?a?a?a?a?a?a?a?a [8]
Guess.Queue......: 1/5 (20.00%)
Speed.#1.........:  4654.7 MH/s (12.22ms) @ Accel:512 Loops:128 Thr:64 Vec:1
Speed.#3.........:   205.8 MH/s (14.87ms) @ Accel:1024 Loops:256 Thr:1 Vec:8
Speed.#*.........:  4860.5 MH/s
Recovered........: 1/1 (100.00%) Digests
Progress.........: 423514525696/6634204312890625 (0.01%)
Rejected.........: 0/423514525696 (0.00%)
Restore.Point....: 458752/7737809375 (0.01%)
Restore.Sub.#1...: Salt:0 Amplifier:36096-36224 Iteration:0-128
Restore.Sub.#3...: Salt:0 Amplifier:247296-247552 Iteration:0-256
Candidate.Engine.: Device Generator
Candidates.#1....: >}+@&@12 -> 892Z5988
Candidates.#3....: t@ffcBER -> ,\iWXdan
Hardware.Mon.#1..: Temp: 61c Util: 98% Core:1365MHz Mem:5000MHz Bus:8
Hardware.Mon.#3..: Temp: 50c Util: 33%

Started: Sat Feb  4 13:37:01 2023
Stopped: Sat Feb  4 13:38:33 2023

```

**Note:** -D is to specify device to use for bruteforce by default hashcat uses gpu you can also use cpu along with gpu by using -D 1,2 option where 2 is your cpu 

Now let's try a different password for example `password@123`, here it gets tricky as this password has 12 char if we try the same command it will first bruteforce all 8 char password then 9,10,11 at last 12 let's calculate the time it will take to bruteforce 9 char password with my hardware hashcat tells that it will take "85 days, 10 hours" to try all 9 char password combination then 16 years, 177 days to try all 10 char combination.


```bash
> hashcat -a 3 -m 1000 --increment --increment-min 9 --increment-max 12 -1 "abcdedfghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789\!@#$%*&()" q ?1?1?1?1?1?1?1?1?1?1?1 -D 1,2
Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 1478 MB

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => s

Session..........: hashcat
Status...........: Running
Hash.Mode........: 1000 (NTLM)
Hash.Target......: 6de00c52dbabb0e95c074e3006fcf36e
Time.Started.....: Sat Feb  4 14:13:04 2023 (1 sec)
Time.Estimated...: Sun Apr 30 04:43:42 2023 (84 days, 14 hours)
Kernel.Feature...: Pure Kernel
Guess.Mask.......: ?1?1?1?1?1?1?1?1?1 [9]
Guess.Charset....: -1 abcdedfghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%*&(), -2 Undefined, -3 Undefined, -4 Undefined 
Guess.Queue......: 1/3 (33.33%)
Speed.#1.........:  5720.1 MH/s (9.86ms) @ Accel:128 Loops:128 Thr:256 Vec:1
Speed.#3.........:   551.5 MH/s (5.53ms) @ Accel:1024 Loops:256 Thr:1 Vec:8
Speed.#*.........:  6272.9 MH/s
Recovered........: 0/1 (0.00%) Digests
Progress.........: 7599030272/45848500718449031 (0.00%)
Rejected.........: 0/7599030272 (0.00%)
Restore.Point....: 0/128100283921 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:15104-15232 Iteration:0-128
Restore.Sub.#3...: Salt:0 Amplifier:54528-54784 Iteration:0-256
Candidate.Engine.: Device Generator
Candidates.#1....: I)seraner -> CAT32r123
Candidates.#3....: sYKvepane -> D!1p7ess1
Hardware.Mon.#1..: Temp: 70c Util: 98% Core:1695MHz Mem:6000MHz Bus:8
Hardware.Mon.#3..: Temp: 87c Util: 95%
```

```bash
> hashcat -a 3 -m 1000 --increment --increment-min 10 --increment-max 12 -1 "abcdedfghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789\!@#$%*&()" q ?1?1?1?1?1?1?1?1?1?1?1 -D 1,2
Session..........: hashcat                                
Status...........: Quit
Hash.Mode........: 1000 (NTLM)
Hash.Target......: 6de00c52dbabb0e95c074e3006fcf36e
Time.Started.....: Sat Feb  4 14:13:48 2023 (30 secs)
Time.Estimated...: Sat Dec 31 10:55:42 2039 (16 years, 329 days)
Kernel.Feature...: Pure Kernel
Guess.Mask.......: ?1?1?1?1?1?1?1?1?1?1 [10]
Guess.Charset....: -1 abcdedfghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%*&(), -2 Undefined, -3 Undefined, -4 Undefined 
Guess.Queue......: 1/2 (50.00%)
Speed.#1.........:  5641.6 MH/s (10.38ms) @ Accel:128 Loops:128 Thr:256 Vec:1
Speed.#3.........:   461.0 MH/s (3.14ms) @ Accel:1024 Loops:128 Thr:1 Vec:8
Speed.#*.........:  6102.6 MH/s
Recovered........: 0/1 (0.00%) Digests
Progress.........: 185152172032/3255243551009881201 (0.00%)
Rejected.........: 0/185152172032 (0.00%)
Restore.Point....: 458752/9095120158391 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:13184-13312 Iteration:0-128
Restore.Sub.#3...: Salt:0 Amplifier:139776-139904 Iteration:0-128
Candidate.Engine.: Device Generator
Candidates.#1....: H72Tf98899 -> SFCXgSTAND
Candidates.#3....: G6oIQHINER -> zLJRg98899
Hardware.Mon.#1..: Temp: 85c Util: 98% Core:1620MHz Mem:6000MHz Bus:8
Hardware.Mon.#3..: Temp: 95c Util: 93%
```

let's try again to crack the same password using some popular wordlist for example rockyou.txt and it is cracked in milli seconds :]

```bash
> hashcat -a 0 -m 1000 q /usr/share/wordlists/rockyou.txt
Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

6de00c52dbabb0e95c074e3006fcf36e:password@123             
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1000 (NTLM)
Hash.Target......: 6de00c52dbabb0e95c074e3006fcf36e
Time.Started.....: Sat Feb  4 14:17:40 2023 (0 secs)
Time.Estimated...: Sat Feb  4 14:17:40 2023 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........: 17694.3 kH/s (3.37ms) @ Accel:2048 Loops:1 Thr:32 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 917504/14344385 (6.40%)
Rejected.........: 0/917504 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> jam183
Hardware.Mon.#1..: Temp: 49c Util:  0% Core:1470MHz Mem:6000MHz Bus:8

Started: Sat Feb  4 14:17:39 2023
Stopped: Sat Feb  4 14:17:41 2023
```

but not all password are in the wordlists so this time we will try to guess password of user Miller who is using password `Miller@2023` let's try to guess chars that can be used by him in password based on his name "Mmiller@!123407" and try all combination between 8 to 12 length and it is cracked in 2 minutes.

```bash
> hashcat -a 3 -m 1000 --increment --increment-min 8 --increment-max 12 -1 "Mmiller@\!123407" q ?1?1?1?1?1?1?1?1?1?1?1 -D 1,2
Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 1478 MB

Approaching final keyspace - workload adjusted.       

Session..........: hashcat                                
Status...........: Exhausted
Hash.Mode........: 1000 (NTLM)
Hash.Target......: facca05d11d88a6e16dd6a9b294654b7
Time.Started.....: Sat Feb  4 14:28:03 2023 (51 secs)
Time.Estimated...: Sat Feb  4 14:28:54 2023 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Mask.......: ?1?1?1?1?1?1?1?1?1?1 [10]
Guess.Charset....: -1 Mmiller@!123407, -2 Undefined, -3 Undefined, -4 Undefined 
Guess.Queue......: 3/4 (75.00%)
Speed.#1.........:  5110.7 MH/s (0.44ms) @ Accel:512 Loops:128 Thr:64 Vec:1
Speed.#3.........:   193.8 MH/s (0.18ms) @ Accel:1024 Loops:256 Thr:1 Vec:8
Speed.#*.........:  5304.5 MH/s
Recovered........: 0/1 (0.00%) Digests
Progress.........: 289254654976/289254654976 (100.00%)
Rejected.........: 0/289254654976 (0.00%)
Restore.Point....: 105405452/105413504 (99.99%)
Restore.Sub.#1...: Salt:0 Amplifier:2688-2744 Iteration:0-128
Restore.Sub.#3...: Salt:0 Amplifier:2560-2744 Iteration:0-256
Candidate.Engine.: Device Generator
Candidates.#1....: m7!@er!MrM -> !7!!MlMlMl
Candidates.#3....: @@74mM@MrM -> !7!M!!lMrM
Hardware.Mon.#1..: Temp: 85c Util: 93% Core:1740MHz Mem:6000MHz Bus:8
Hardware.Mon.#3..: Temp: 95c Util: 74%

facca05d11d88a6e16dd6a9b294654b7:Miller@2023
```

So it is not only about how powerfull your hardware is to crack the password, if you have good information about the target and a good wordlist you can crack most password easily.

there is one more concept of rules which I need to explore.

Read more:
Wiki: https://hashcat.net/wiki/
Mask Attack: https://hashcat.net/wiki/doku.php?id=mask_attack
Rule Based Attack: https://hashcat.net/wiki/doku.php?id=rule_based_attack
