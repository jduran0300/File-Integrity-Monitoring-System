# File-Integrity-Monitoring-System

The diagram below displays an overview of the File Integrity Monitoring System: 

![](https://lh6.googleusercontent.com/LUAoTVql-FKJXzh4Zh8QuB7_6271r2TiZCJ-Fxmo6rJF71-f-UKX4aD5H7qS4bf116gJWFaFwRm2Hj_HaHNdFeQ2X_7KKlRxzxzL5jzIke5o0rsItubd_-tsiMFq6bOAWKkMPHnZavSjDlF33PXmIHo)

**Code Overview**

In order to create the FIM program, a script was created using Powershell Integrated Scripting Environment, a graphical user interface used for writing, editing, and testing Powershell scripts and commands.

![](https://lh3.googleusercontent.com/PZ1Ch50ySnQfzG7QJNcI2NCt59hYre_UdD9wIZ3TB-aShchTe_GXT7XX74nb1KD_iRinILVNjIBvK9qOqXbsI8a_h1jpMofIzG2F90STqSJ3Hyne252fMCUpendnhirFIuJg6I2H_ORV9gbiXG-LJt8)

The Calculate-File-Hash function in the code snippet uses the Get-FileHash cmdlet to compute a hash value for a specified file. The -Path option indicates the file's location, and the -Algorithm option specifies the cryptographic algorithm used for hashing, such as SHA-512. The function returns an object containing details about the calculated hash, which can be used for various file integrity and comparison tasks.

![](https://lh6.googleusercontent.com/8m89-a_J99zEUGAeX3cAF5lYZwbaVzJtIWYEDMyMKl_Cpa3gie6xvEWhY3Pp65nnM8Si02N11rUIL-Jk4lob_ZLlKybRIoAlonngvdFrgyPuZjkxkDs9wp611M5CEby0gzPNOfqXioGI7JoV2jR4wW8)

The Erase-Baseline-If-Already-Exists function employs the Test-Path cmdlet to determine if the entered file path ending with baseline.txt exists in the current directory. The result of this check is stored in the variable $baselineExists, which becomes true if the file path is found or false if it isn't. If $baselineExists is true, the function proceeds to remove the existing baseline.txt file using the Remove-Item cmdlet. This function ensures that any pre-existing baseline is cleared before initializing a new one, offering a clean slate for collecting or monitoring file hash values.

![](https://lh5.googleusercontent.com/iRvOcERQRQmr6Ph7Gj4fPmPex9KRVbzl9L4II2sAJ2vZbfOgsW8rwFzrEsUdh6c7EqYbmZksj07qBiHOFL8PDYHoOs5OzB-jG37ilTLscu278s45nI8lv-krgpGeaUekPmznAqJSXbsnsVoUAHtIgYY)

This code segment creates an interactive user prompt by displaying messages and capturing the user's response. The \`Write-Host ""\` statements insert blank lines for formatting, while the user is presented with options to either collect a new baseline or begin monitoring files using a saved baseline. The user's choice is read using \`Read-Host -Prompt "Please enter 'A' or 'B'"\`, and the entered response is stored in \`$response\` for subsequent script actions.

![](https://lh5.googleusercontent.com/aygu9WI8bEfnWxXGfuTNO_5j17x6AN7HMzPk45v-_F6vJIDMyuidveqv-EIs1fdOjgX5iJIyEVenkckcLlzj7iwCcrxXF2XxFnJzV83LWvvexZ8lYBC2PBbunWkIF30dTSncoR_eAQD5Pomd3_GEAx8)

In this section of the code, the script responds to the user's choice 'A' by executing a sequence of actions. If the user's response is equal to "A" (regardless of the case), the script proceeds to perform the following steps:

1\. It first invokes the \`Erase-Baseline-If-Already-Exists\` function, which checks if a file named \`baseline.txt\` exists and deletes it if it does.

2\. Then, the script collects a list of files in the \`.\Files\` directory using the \`Get-ChildItem\` cmdlet and assigns them to the variable \`$files\`.

3\. For each file in the \`$files\` collection, the script calculates the hash using the \`Calculate-File-Hash\` function, passing the file's full path to the function. The hash is then appended to the \`baseline.txt\` file, along with the file's path, using the \`Out-File\` cmdlet with the \`-Append\` parameter. 

Overall, this portion of the script deletes any existing baseline, calculates the hash values for files in the target directory, and stores the hash-file pairs in the \`baseline.txt\` file to establish a new baseline for comparison in subsequent steps.

![](https://lh5.googleusercontent.com/YBYkGjuhsTB-4qR2WtgglKBqMXuUHKpryo9SFwpBnUhP4WeXiBokVShOrKYuqfuL12mUms4XBVyKCVq2lf_2UbZYCjfDBW-32J8Hj-eKsQDrr66DesNeMZb_0FLtixdB7NYLkQm2OWDS9SoDLRLjmuI)

This section of the code addresses the situation where the user selects the option to begin monitoring files with a saved baseline (\`B\`). It is indicated by the condition \`elseif ($response -eq "B".ToUpper()) \`. In this case, the script initializes an empty dictionary named \`$fileHashDictionary\` to serve as a container for associating file paths with their corresponding hash values.

The next step involves loading the content of the "baseline.txt" file, which contains pairs of file paths and hash values. This content is stored in the variable \`$filePathsAndHashes\` using the \`Get-Content\` cmdlet with the \`-Path .\baseline.txt\` argument. 

Subsequently, a loop is implemented to iterate through each line of the loaded content. Within this loop, the script splits each line using the pipe character (\`|\`) as a delimiter, resulting in an array containing the file path and hash value. The file path (located at index 0) is then used as the key, while the hash value (index 1) is designated as the corresponding value. This key-value pair is added to the \`$fileHashDictionary\` using the \`.add()\` method.

In summary, this code segment establishes the foundation for file monitoring with a baseline by extracting file paths and hash values from the "baseline.txt" file, organizing them into a dictionary, and preparing them for comparison during the subsequent monitoring process.

![](https://lh4.googleusercontent.com/QWZA7WCM-LkvDaOPiL1bgf5yXwvKTfzOxDZFFkOe8HcBgEbQvSAybWSs2Q2mdUkRKl7nqr8kds2USM9dS6vLuyxT4lp8eAZ5JoAtPbDwEZnQ8yx6Rb3zMEp4KzInmAQfNytvMGYlM8JVhXfATrTjdDM)

This code segment orchestrates an ongoing monitoring process for files using a previously established baseline. It initiates a perpetual loop with \`while ($true)\` to ensure persistent monitoring. Within each loop iteration, a brief pause of one second is introduced using \`Start-Sleep\` to regulate the monitoring frequency.

As the loop progresses, the script employs the \`Get-ChildItem\` cmdlet to compile a list of all files within a designated directory (.\Files). This list is captured in the variable \`$files\`.

Subsequently, a \`foreach\` loop is utilized to sequentially process each file in the list. During this traversal, the script calculates the hash value of each file using the custom function \`Calculate-File-Hash\`, which is supplied with the complete file path (\`$f.FullName\`) as input.

The script cross-references the calculated hash values with those stored in the \`$fileHashDictionary\`. If a hash value is absent (indicating the file is new and not present in the baseline), the user is promptly alerted through a notification displayed using the \`Write-Host\` cmdlet. This notification is highlighted with green text (\`-ForegroundColor Green\`) to emphasize the detection of a new file creation.

Essentially, this section sets up a recurring procedure that consistently computes hash values for files in the monitored directory. It promptly notifies the user of any new file creations by detecting the absence of hash values in the established baseline.

![](https://lh5.googleusercontent.com/ZSBj51pSfI80bk5qtlWqhSiQVQAkrhVi40bBWXjoQTEDEl4pE5J-ybG1N6paX9c_MsnciS93GkTK6cJ_SO_BwtAHZ5RCQnIrYtXr9tAT2tTPc3STPnamZsiOC45qODAWJ1QqPPnfm2hTg2PESE1FpqQ)

In this segment, an additional layer of file integrity monitoring is established. Within the \`else\` section, the script checks whether the calculated hash value of a file matches the hash value stored in the \`$fileHashDictionary\`. If they match, it signifies that the file has not been altered since the baseline was created, and no action is taken. However, if the hash values differ, it indicates that the file has been modified.

In the case of a hash value mismatch, the script employs the \`Write-Host\` cmdlet to promptly alert the user about the potential compromise. The alert message, displayed in yellow text (\`-ForegroundColor Yellow\`), informs the user that a specific file, denoted by \`$hash.Path\`, has been changed. This mechanism serves as an early warning system, allowing users to detect unauthorized modifications to files within the monitored directory and take appropriate actions to investigate and mitigate any security breaches.

By incorporating this logic, the script enhances its capability to identify and promptly notify users of unauthorized file alterations, thereby strengthening the overall security posture and aiding in the preservation of data integrity.

![](https://lh6.googleusercontent.com/EQAWKUtCeY5_p7VPo2MtfTKkwS8PXghT21lo3yz_l7SMHwCmRlHOexT3YOz0T3oltIYMCKFVOR_8NsnXYpQxpTzB28KlUZGFoWjzpuijO96E9UsbULGYcKyHEB3cAYrMyvMFSldfaYC2soxcYKsi_iM)

Within this section of the script, a \`foreach\` loop iterates through the keys of the \`$fileHashDictionary\`, which contains the file paths as keys and their corresponding hash values as values. For each key (file path), the script employs the \`Test-Path\` cmdlet to determine if the file still exists at the specified path. The result of this check is stored in the variable \`$baselineFileStillExists\`.

If the check reveals that the file no longer exists at the designated path (\`-Not $baselineFileStillExists\`), it implies that one of the baseline files has been deleted from the monitored directory. In response, the script utilizes the \`Write-Host\` cmdlet to generate a notification message that alerts the user about the deletion of the specific file. The message is displayed with dark red text (\`-ForegroundColor DarkRed\`) against a gray background (\`-BackgroundColor Gray\`), ensuring its prominence and providing a visual cue that signifies a potentially significant change.

This component contributes to the script's comprehensive monitoring and notification mechanism, extending beyond the detection of new or modified files. It addresses the scenario where baseline files have been removed from the monitored directory, enabling users to stay informed about any unexpected file deletions and take appropriate actions as needed to investigate the cause and mitigate any potential security implications.

**Program Testing**

This program has several capabilities, such as calculating file hashes, baseline establishment, baseline monitoring, detection of new file creations, detection of file modifications, and file deletion detection. Each of these functions will be tested. 

**Calculating hashes and baseline establishment**

![](https://lh6.googleusercontent.com/4dZkzJFjD4WJO7e3GKwjBSkCWJCTfJ_9bKMsNC1W0it3OFo0nxAks0x6Cw-i0qgyjWXdWIydBLcJSs_2NsFTUni5oBG9jeHcI9CJ79SFFPEajNvG7vWcjJ58Na2ps0dY0aoOYyq9uHTwt30cXcc25KM)

When the File integrity monitoring file is run, we are prompted with the option to collect a new baseline or begin monitoring the files that already have a saved baseline. For this test, we will choose choice A.

![](https://lh5.googleusercontent.com/edPQvpLRGYaqsfAQ-NPAdM8beD4y4WQDiQIc1P-Jd4NthI_PcsmjTE3kmTggKs8etEVoQDV7Ip9IBQSBkUazn5rIcJrgwrQapSkNBOL0LUO5ZMFPfkJcT7XqmvsB-DfYlWI6HgCWJ4PQBSBJe6vxhfU)

In our Desktop directory, a new file has been created- baseline.txt. This is where baselines will be established.

![](https://lh6.googleusercontent.com/Q9U3saDJLs4SqYMIW_Rrqd4jQ8R5PPnwsoZtJ99ltRi6LwN6KDtK7pVh1b486SdtaUosO16rB72vQjgT2bH82vuOio86TuUAu9Jr5VDmp3pX7Uf7umslIOuyKYiQ9jVqA2NrETO_5p09LJ4s8yNuUhM)

Within this text document, we can find the file paths and corresponding hash values for the two files within the Files folder, confirming that the hash calculation portion of the program is working. 

To further test the hash calculation functionality, the contents of the first file will be edited.

![](https://lh3.googleusercontent.com/6FE4y1sz7oXO1_dRSSvIDQC72T4geUVv0l59rEiPNdyk28xmJ4-Zwav0uJTAkRXONpgp14EAMndBNXAz3R10stuphwBzwdvN3_gAG9kdv9OGQtZSrEzpiUZHv1jPt9Bkm0sfE4zr98fUNbcEu1IXcOI)

It can be seen that the hash value corresponding to the first file has changed due to the modification of the file’s contents. This is further confirmation that the program’s ability to calculate unique hash values for a file is working properly. 

**Baseline Monitoring/Detection of File Modifications**

To test baseline monitoring, we will rerun the program and choose choice ‘B’, which opts for monitoring files with saved baselines. 

![](https://lh6.googleusercontent.com/YiuXDggokfjs9_bHo11zcBr9w7cGizymrUZNkX4nvbbuU4f5fDn1q9snfGdEWdb4BWuJMa9nOFiWdhRYKkgGL93j5Whzrd8rj1cjXqDcWtOVWH8SXy4bjVY9CHOAf74qx5sJC-ldRggUWo1SOXwi6go)

After this is done, no alerts have been created, as there have not been any modifications to the contents of the monitored files since their last baseline collection. 

![](https://lh5.googleusercontent.com/PHZ65Ea9P3z32hu_vfrAzSshbjdHJMmEu7sDewnP2U_61d6iaNTfygCrsiuZ-yA8FFDuNXgW5k5P0aD-e7FEtZJRdIZtOOYd_dMqQTDuh71Y4pDh4MqTW_9m5J5UpJQRK2evstyw8EwCevRFIDFK5Yw)

However, when the contents of the first file is changed, alerts are sent in the console informing the user that the file has been modified, confirming that the program’s ability to monitor baselines is working properly. 

**Detection of New File Creations**

When a new file is created in the Files folder, alerts are sent to the user that a new file was added to the monitored file. This alert also includes the path of the file, confirming that the detection of new file creations functionality of this program is working. 

![](https://lh5.googleusercontent.com/JaHk606Fkaw3DbsUvKx3_gTwfS7OMoIrjyrI0LYqnaO0u0hwkxAJwIqV70eGhr5-nvz87iivfhlhuZRb7E1VCjtjz8gDKwADqcm00QjcQlinAF9qS6vEeszQGJYlkqBlgsy-d6gboa7b5wFlWy2gxo0)

![](https://lh4.googleusercontent.com/-7-GV2G7Kf-a7kVCVoSk6XmWhOe3ROY3Ui6YgrFxFpYX8vzEhIazg9zVfIBQv0j5pdy0CBm5Ln-CQ7fYejDw2irgsPWTLye9vePM2n3XcAKsy8Wz9nALHdQjPsxRL-t5WSeQmeKhedf5bQ1NMAZPhHc)

**Detection of File Deletions**

![](https://lh3.googleusercontent.com/v_Q62s6w9vINZle79XZZtMzTBB0NeJLHZyTzCG8EWWrPlv3AVqaoJhTPOw9dMI9M0DvtR6oCOg39ihHe4biICsA7xzr1mhAw0RuiU-xqIUoDm3lSAc4xYoVX2-ncgY512YIde8--7WLWiZBb7mxYR4U)

![](https://lh5.googleusercontent.com/544B1HOqj_7XHH3V-yCQHVJigfcjE_Stg-W_x0MmNhNQdgWSXMpsm-3pEx8eHAhvIw-osA8yg2aG6wISfd5H0iNmr61x_nlLMdgSaJqJ-Qz3_a5kfOa0EIubo9GnPFtjyA60Dc059ZHp1B1KYFTDyI4)

When the second file in the Files folder is deleted, an alert is sent to the user, telling them that a file has been deleted and providing the path of the deleted file. This notification confirms that the detection of file deletions portion of this program is working properly.

**Project Summary:**

This File Integrity Monitoring System employs PowerShell ISE to offer robust file integrity management. Calculate-File-Hash computes cryptographic hash values for files. Erase-Baseline-If-Already-Exists ensures a clean slate for baseline creation. Interactive prompts guide user actions. New baseline creation involves hash calculation and storage in "baseline.txt". Monitoring detects new files, modifications, and deletions, providing real-time alerts. This system enhances file security and integrity measures, offering comprehensive file monitoring and notification capabilities. Improvements could be made to this system through some error checking, flow control, and integration with Twilio API to send a direct message to the user about a change in a file’s integrity rather than just having the notification arrive on the console. Thank you for reading!
