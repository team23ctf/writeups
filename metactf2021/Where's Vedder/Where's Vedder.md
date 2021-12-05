# MetaCTF 2021
## Where's Vedder (525pts)
### Description: 
>Please help find our dear friend Vedder Casyn. His last known location was [this location](https://user-images.githubusercontent.com/43623870/144760334-76c9b328-8b05-49dd-ba0a-07d2e45b9e50.jpg). We believe it's within a public forest area in his home state.
The answer should represent the MD5 hash of the address of the location. For example, if the address is: "150 Greenwich St, New York, NY 10006" then the flag will be e8244cb2f4d53117e9797af909123e86. Make sure to have the address format the same as above.
[This video](https://www.youtube.com/watch?v=RoqWbpZUOSo) is helpful for thinking about the right way.
### Included Files:
> [location.jpg](https://github.com/zylideum/writeups/blob/main/metactf2021/Where's%20Vedder%3F/location.jpg)

### Step 1 - Observations:
This challenge does not have any gimmicks. We started by trying to identify metadata for information on location or anything we could go off of. Unfortunately, it was scrubbed and there is truly nothing more than this image.
We do know that it's in his home state, and near a public forest area. We also know that it's a business given the parking lot.

I know many people reading this are looking for a trick to find it, but there really is nothing more than identifying the type of building it is and manually going through Google Maps to find it.
The sign with a number on it is also meaningless information, in fact it's a red herring.
<br /> <br />
![image](https://user-images.githubusercontent.com/43623870/144760446-633a6e16-4164-49b1-bcba-4a62a2df9838.png)

(It looks like 1992, but the address has nothing to do with 1992. It was probably the established date.)

### Step 2 - Identification:
Based on the vans, the style of building, and the fact that there's a seemingly residential property on the business land - we came to the conclusion that this was a funeral home, morgue, or mortuary of some sort.
We scoured Indiana for every one of these businesses we could find.

### Step 3 - Findings:
Again, there really is no trick. We scoured the funeral homes by hand and eventually found it.
<br /> <br />
![image](https://user-images.githubusercontent.com/43623870/144760696-09385cf2-3e4f-4a84-8a4a-7817b1c39a7f.png)
![image](https://user-images.githubusercontent.com/43623870/144760707-f5dbe3ec-56f0-4e26-bb9f-8569e2818c99.png)

Denbo Funeral Home in southern Indiana was our hit.

### Step 4 - Flag Generation
The challenge specifically wants a hash of the standard address, including postal code, city, and address.
We directly copied the address from Google Maps, hashed it, and entered the flag.

<details>
  <summary>Flag Spoiler</summary>
  7be0798af71f79eadb9254d3554aa301
 </details>

## Learning Takeaways
I wish I could say I learned how to use some cool metadata tool, or some kind of triangulation method, but this honestly came down to pure persistence and focus. I learned a couple of things in the byproduct of searching, like how to look at business records, but overall this was an extensive manual search.
