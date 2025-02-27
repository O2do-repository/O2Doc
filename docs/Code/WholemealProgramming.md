---
title: Wholemeal Programming
nav_order: 2
last_modified_date: 27-02-2025
---

# Wholemeal Programming

Wholemeal programming is a style of programming that emphasizes programming with entire data structures rather than individual elements.
It is a style that is often used in functional programming, where you map functions on lists, sets, etc.
It is a way to avoid loops and to make the code more readable and more concise.

It avoids duplicating the logic, and it is a way to separate the business logic from the data manipulation.
As your function is pure, you can test it easily and you can reuse it in other parts of your code.

The idea is to define the elementary function that you need to apply to every element of a list, and then to map this function on the list.
As such, you never write loops, your logic is in the elementary function, and you can reuse it easily with very little probablity of errors.

Here is an example using Haskell to show you how it works.

## Let's define the data
```hs
-- A skill is an enum
data Skill = BA | Archi | Dev | PM

-- A profile is a name and a main skill
data Profile = Profile { profileName :: String, profilMainSkill :: Skill }

-- A RFP is request for a main skill
data RFP = RFP { jobTitle :: String, rfpMainSkill :: Skill }

-- A Matching is a score assigned to the combination of a profile and a RFP
-- The number of matchings to check is equal to the cartesian product of profiles and rfps => #matchings = #profils x #rfps
-- But you only have to calculate them once... As the result is always the same for the same profile and the same RFP (it is a pure function)
data Matching = Matching { 
    profil :: Profile, 
    rfp :: RFP, 
    score :: Int }
```

## Now the logic

Now let's define the elementary function where the business logic sits:
For one profile and one RFP, returns the matching (which is the score associated to it)
```hs
matchingForOneProfileAndOneRFP :: Profile -> RFP -> Matching
matchingForOneProfileAndOneRFP = undefined -- To be implemented: This is where your business logic is: the only place where you need to put the rules for scoring a match between a profile and a RFP
```

in C# it would be equivalent to 

```c#
public Matching matchingForProfile(Profil profil, RFP rfp) {}
```

When you have a new profile, you have to check it against all RFP that you have in store already.  
This is simply done by mapping the elementary function to every element of the list of RFPs
It will return a list of matching: one for each RFP in the input list

```hs
matchingsForOneProfileAndManyRFP :: Profile -> [RFP] -> [Matching]
matchingsForOneProfileAndManyRFP profile = map (matchingForOneProfileAndOneRFP profile)
```

in C# it would be equivalent to 

```C# 
public List<Matching> matchingsForProfile(Profil profil, List<RFP> rfps) {
     return rfps.Select(rfp => matchingForProfile(profil, rfp));
}
``` 

You map the function with a fixed profile on every element of the list of RFPs. 

If you can calculate a matching for one profile and one RFP, then you can calculate a matching for one RFP and one profile
Note that in Haskell, order of arguments is important (positional arguments)
In C# you would use named arguments so you don't care about this. 

```hs
matchingForOneRFPandOneProfile :: RFP -> Profile -> Matching
matchingForOneRFPandOneProfile rfp profile = matchingForOneProfileAndOneRFP profile rfp
``` 

If you have one new RFP, you can check the matching against all profiles you have in store.  
Same principle as before.  

```hs 
matchingsForOneRFPAndManyProfiles :: RFP -> [Profile] -> [Matching]
matchingsForOneRFPAndManyProfiles rfp = map (matchingForOneRFPandOneProfile rfp)
``` 

in C# it would be equivalent to 

```C#
public List<Matching> matchingsForProfile(RFP rfp, List<Profil> profils) {
    return profils.Select(profile => matchingForProfile(profil, rfp));
}
```

Note the similarities with the previous function where we had one profile and many rfp. 
Now you map the same function but this time on a list of profiles and with a fixed rfp.  


You don't need, in your app, to calculate the matching for all the profiles and all the RFPs.
The reason is that it would take too much time and resources as you have #rfp x #profils calculations to do... 
It is much better to do it "au fil de l'eau" when new items (profils or rfps) are added individually.
However, I want to show you that it would be very easy to do it if you wanted to. 
You would just have to map the previous function on all the profiles and all the rfps.

```hs
findAllMatchings :: [Profile] -> [RFP] -> [Matching]
findAllMatchings profils rfps = concatMap (\p -> matchingsForOneProfileAndManyRFP p rfps) profils
``` 

But we could do also 

```hs 
findAllMatchings profils rfps = concatMap (\r -> matchingsForOneRFPAndManyProfiles r profils) rfps
```

It would be the same.  

in C# it would be equivalent to : 

```C#
public List<Matching> findAllMatchings(List<Profil> profils, List<RFP> rfps) {
    return profils.Select(p => matchingsForProfile(p, rfps));
}
```

## Conclusion

This is just a very simple but very frequent example of wholemeal programming.  It is about the style of programming we use, instead of jumping into writing loops and manipulating data structure rather than treating them as whole. 
Functional programming helps thinking on this because it has done much better introspection on abstracting patterns like `map`, `filter`, `fold`.  

### Note

This code hasn't been checked formally speaking, it may be bugged, but the logic is there.  Don't hesitate to contribute.  