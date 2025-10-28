---
title: " Attacker Heatmap"
classes: wide
header:
  teaser: /assets/images/digital-forensics/attacker-heatmap/mitre-attack.png
ribbon:
description: " Create a full picture of building a strong detection environment based on the attacker's behavior."
categories:
  - digital-forensics
toc: true
---

As a detection engineering, our job is based on the SOC-CMM Framework, separated into two main categories:

1- Use Case Management   
2- Detection Engineering & Validation

We will discuss use case management, which is responsible for creating threat profiling for the organization (the client in the MSSP context) and other responsibilities. However, we will now focus on threat profiling.

[Threat intelligence plays an important role in identifying the most relevant attackers]

## Threat Profiling

We know that we are going to explain how we can make the attacker suffer and take a baby step to create a full picture of building a strong detection environment based on the attacker's behavior.

threat profiling, which is also known as attacker heatmap, separated into:

Strategic: which is responsible for gathering information about any threat actors interested in our client

operational: which is responsible for how many of them is active and what sector intersted in.

tactical: here we are focusing on which TTPs(tactics, techniques, and procedures) are used by the attacker and giving it a score based on opertional stage(in our case, activity and sectors).

So, what is the score? The score is a number we give to techniques as a priority to which one should focus on first in our detection rules and playbooks, trying to cover every possible technique from the highest one to the lowest.

## Example



Our program is focusing on the targeted jordan government and telecommunications.



We are beginning with strategic and operational as we discussed.



By Using [ETDA](https://apt.etda.or.th/cgi-bin/aptsearch.cgi), [APTMAP](https://andreacristaldi.github.io/APTmap/) and [Exel sheat](https://docs.google.com/spreadsheets/d/1H9_xaxQHpWaa4O_Son4Gx0YOIzlcBWMsdvePFX68EKU/edit?gid=1636225066#gid=1636225066)



We can get:   

All groups targeting Jordan and targeting the sector Telecommunications and government (using ETDR)



```

Changed Name            Country   Observed           APT groups

Chafer, APT 39          Iran      2014-Sep 2020      target telecommunication and government

DNSpionage              Iran      2019-Apr 2019      target telecommunication and government

Earth Krahang           China     2022               target telecommunication and government

Magic Hound, APT 35, 
Cobalt Illusion,

Charming Kitten         Iran      2012-Jun 2025      Active(target telecommunication and government)

Molerats, Extreme Jackal,                 
Gaza Cybergang          [Gaza]    2012-Jul 2023      Active(target telecommunication and government)

MuddyWater, Seedworm, 
TEMP.Zagros,

Static Kitten           Iran      2017-Jul 2025      Active(target telecommunication and government)

OilRig, APT 34, Helix
Kitten, Chrysene        Iran      2014-Sep 2024      Active(target telecommunication and government)

Operation HangOver, 
Monsoon, Viceroy Tiger  India     2010-Jan 2025      Active(target telecommunication and government) 

Operation Parliament    [Unknown] 2017               target telecommunication and government

Sea Turtle              Turkey    2017-2021          target telecommunication and government

```


All groups targeting Jordan and targeting the sector Government only (using ETDR)
```

Changed Name           Country      Observed        APT groups

CopyKittens,
Slayer Kitten          Iran         2013-Jan 2017    

Desert Falcons         [Gaza]       2011-Oct 2023   Active

Gelsemium              China        2014-2023       Active

Inception Framework,
Cloud Atlas            Russia       2012-2024       Active

NetTraveler, APT 21, 
Hammer Panda           China        2004-Dec 2015    

Silence, Contract Crew [Unknown]    2016-Aug 2022    

Sofacy, APT 28, 
Fancy Bear, Sednit     Russia       2004-Apr 2025   Active

Turla, Waterbug, 
Venomous Bear          Russia       1996-2024       Active 

Volatile Cedar         Lebanon      2012-Early 2020  

WIRTE Group            Middle East  2018-Feb 2024   Active 



```

  ![error](/assets/images/digital-forensics/attacker-heatmap/APT-groups.png)



All groups targeting Jordan and targeting the sector Government, and  (using APTMAP)



```

changed name: 

APT21,APT 21, APT21, HAMMER PANDA, Hammer Panda, NetTraveler, TEMP.Zhenbao (low)

```


  ![error](/assets/images/digital-forensics/attacker-heatmap/APT-groups-china.png)




All groups targeting Jordan and targeting Telecommunication and government sectors (using APTMAP)



```

changed name:



Active(target telecommunication and government):

APT35,APT 35, APT35, Ballistic Bobcat, COBALT MIRAGE, Charming Kitten, CharmingCypress, Cobalt Illusion, Educated Manticore, G0059, Magic Hound, Mint Sandstorm, Newscaster Team, Operation “BadBlood”, Operation “Sponsoring Access”, Operation “SpoofedScholars”, Operation “Thamar Reservoir”, Phosphorus, TA453, TEMP.Beanie, Tarh Andishan, Timberworm, TunnelVision, UNC788, Yellow Garuda,

```

  ![error](/assets/images/digital-forensics/attacker-heatmap/APT-groups-iran.png)



All groups targeting Jordan and the targeting sector (using Excel Sheet)

```

Changed name: 

target government

- Sofacy    APT28   Sednit  Pawn Storm  Group 74    Tsar Team   Fancy Bear  Strontium   Swallowtail SIG40   Grizzly Steppe  TG-4127 SNAKEMACKEREL,Armada Collective, Dark Power, G0007, ATK5,Fighting Ursa, ITG05,Blue Athena 

- AridViper Desert Falcon   APT-C-23    Two-tailed Scorpion APT_C_23(low)

- BlackOasis (low)

```

  ![error](/assets/images/digital-forensics/attacker-heatmap/APT-groups-exel.png)


After we gather this information can now go to the theired step, which is tectical.



For creating the heatmap, we should use MITER, We are going to use a tool from MITRE called [Attack Navigator](https://mitre-attack.github.io/attack-navigator/).


based on the score we gave to the techniques from all APTs and collected all of them in one heat map, this is the final one. 

  ![error](/assets/images/digital-forensics/attacker-heatmap/mitre-attack.png)








