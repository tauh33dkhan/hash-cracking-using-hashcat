I did some testing to crack NTLM hash without any prior knowledge of password policy I used all small case,uppercase and special characters (!@#$%^&*) to crack hash of "password"  which has 8 char as we don't know the password length we can try to bruteforce it by trying all combination of 8 char password then 9,10,12 so when I tried it hashcat showed that it will take 1 day 6 hours to try all combination but cracked it in about 3 sec with speed of 5900 MH/S.


Now let's try some different password for example password@123 now here it gets tricky as this password has 12 char now if we try the same command it will first bruteforce all 8 char password then 9,10,11 at last 12 let's calculate the time it will take to bruteforce 9 char password with my hardware hashcat tells that it will take "85 days, 10 hours" to try all 9 char password then 16 years, 177 days to try all 10 char combination.

Now let's again try bruteforce the same password using some popular wordlist for example rockyou.txt and it is cracked in milli seconds :]

but not all password are in the wordlists so this time we will try to guess password of Miller who is using password Miller@2023 now let's try guess chars that can be used by him in password based on his name so we have "Mmiller@!123407" and we will again try all combination between 8 to 12 length and it is cracked in 2 minutes
