# My Aquarium – DawgCTF 2025
## Reconnaissance
The challenge, **My Aquarium**, instructed us to discover a "secret favorite animal" hidden within a website. Initial manual exploration of the site revealed three main pages:
- A quiz page to "find your favorite animal"
- A gallery page displaying sea animal pictures
- A fact page with static content

With no immediate indicators of sensitive content, I proceeded to run directory enumeration tools such as **Gobuster** and **Dirb**. However, these scans did not yield any hidden directories or useful paths.

---
## Discovery of Azure Blob Storage
Upon closer inspection of the **Fact Page**, I noticed the URL structure resembled that of an **Azure Blob Storage endpoint**.

![image](https://github.com/user-attachments/assets/7b6d9d88-68df-42e5-8205-e21dd82d4ccf)

This hinted that the content might be stored in a publicly accessible Azure Blob container named `aquarium`.

Azure Blob Storage allows for public container browsing if misconfigured. Specifically, appending the following query string to a container URL may reveal a list of all blobs inside that container:

```
?restype=container&comp=list
```
- `restype=container` tells Azure we're referencing a container (not a single file/blob).
- `comp=list` asks Azure to return a list of all files (blobs) within the container.

---
## Enumeration of Blobs
Appending the parameters to the aquarium container URL:

```
https://onlineaquarium.blob.core.windows.net/aquarium?restype=container&comp=list
```

…returned an XML listing of all blob objects within the container. This blob list revealed the existence of a file that referenced the secret favorite animal.

![image](https://github.com/user-attachments/assets/93fec8fb-ccea-4c38-b476-69fc69654607)

---
## Retrieving the Flag
Now that we can see the objects inside the container, we can find the link to the secret favorite animal.

We go to the provided link and we get the **flag** needed to complete the challenge.
![image](https://github.com/user-attachments/assets/58c05bdf-5682-4cb0-b8c4-7586c0a704e5)
