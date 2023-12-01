# F.O.R.T.値の算出方法

QueryProfileに載っていない、アカウントのF.O.R.T.値を算出する方法。

## 1. サバイバースクワッドとその他の情報を取得して計算する。

まず、リサーチとコマンダーレベル両方のF.O.R.T.値の合計のデータがあるのでそれを取得します。

以下のコードで取得できます。

(dataは取得したQueryProfileです。)

```python
# F.O.R.T.値の初期化
fort = {"fortitude": 0, "offense": 0, "resistance": 0, "technology": 0}

for k, v in items.items():
      templateId = v["templateId"]
      if "Stat:" in templateId and "phoenix" not in templateId:
        if "fortitude" in templateId:
          fort["fortitude"] += v["quantity"]
        elif "offense" in templateId:
          fort["offense"] += v["quantity"]
        elif "resistance" in templateId:
          fort["resistance"] += v["quantity"]
        elif "technology" in templateId:
          fort["technology"] += v["quantity"]
```

次は、サバイバースクワッドに配置した時のサバイバーのパワーの合計を調べる必要があります。

(これがまぁ少し面倒。ゴリ押しで書いたわかりづらい非効率なコードになってると思います。すいません。)

```python

mythic_lead_list = ["kingsly", "noctor", "treky", "jumpy", "raider", "yoglattes", "dragon", "samurai", "tiger", "malcolm", "princess", "ramsie", "birdie", "eagle", "spacebound", "fixer", "flak", "zapps", "countess", "maths", "sobs", "frequency", "rad", "square"]
lead_much = ["doctor", "trainer", "martialartist", "soldier", "explorer", "gadgeteer", "engineer", "inventor"]
squad_mapping = {"doctor": "squad_attribute_medicine_emtsquad", "trainer": "squad_attribute_medicine_trainingteam", "martialartist": "squad_attribute_arms_closeassaultsquad", "soldier": "squad_attribute_arms_fireteamalpha", "explorer": "squad_attribute_scavenging_scoutingparty", "gadgeteer": "squad_attribute_scavenging_gadgeteers", "engineer": "squad_attribute_synthesis_corpsofengineering", "inventor": "squad_attribute_synthesis_thethinktank"}
survivor_squad = {"squad_attribute_medicine_emtsquad": {"leader": None, "teams": [None, None, None, None, None, None, None]}, "squad_attribute_medicine_trainingteam": {"leader": None, "teams": [None, None, None, None, None, None, None]}, "squad_attribute_arms_closeassaultsquad": {"leader": None, "teams": [None, None, None, None, None, None, None]}, "squad_attribute_arms_fireteamalpha": {"leader": None, "teams": [None, None, None, None, None, None, None]}, "squad_attribute_scavenging_scoutingparty": {"leader": None, "teams": [None, None, None, None, None, None, None]}, "squad_attribute_scavenging_gadgeteers": {"leader": None, "teams": [None, None, None, None, None, None, None]}, "squad_attribute_synthesis_corpsofengineering": {"leader": None, "teams": [None, None, None, None, None, None, None]}, "squad_attribute_synthesis_thethinktank": {"leader": None, "teams": [None, None, None, None, None, None, None]}}

base_power_mapping = {"mythic": 25, "legendary": 20, "epic": 9, "rare": 7, "uncommon": 4, "common": 1}
level_constant_mapping = {"lead_survivors": {"mythic": 1.5, "legendary": 1.374, "epic": 1.254, "rare": 1.08, "uncommon": 1}, "survivor": {"mythic": 1.645, "legendary": 1.5, "epic": 1.374, "rare": 1.245, "uncommon": 1.08, "common": 1}}
evolution_constant_mapping = {"lead_survivors": {"mythic": 9, "legendary": 8, "epic": 7, "rare": 6.35, "uncommon": 5}, "survivor": {"mythic": 9.85, "legendary": 9, "epic": 8, "rare": 7, "uncommon": 6.35, "common": 5}}

items = data["profileChanges"][0]["profile"]["items"]

# itemsの中から、サバイバーの情報を取り出す。(レベルやティア。配置されているスクワッドID等)

for key, item in items.items():
  templateId = item["templateId"]
  if "Worker:" in templateId:
    attributes = item["attributes"]
    if "squad_id" in attributes:
      squad_id = attributes["squad_id"]
      if squad_id == '':
        continue
      personality = attributes["personality"]
      level = attributes["level"]
      squad_slot_idx = int(attributes["squad_slot_idx"])

      survivor_id = templateId.replace('Worker:', '')
      if "t05" in survivor_id:
        tier = 5
      elif "t04" in survivor_id:
        tier = 4
      elif "t03" in survivor_id:
        tier = 3
      elif "t02" in survivor_id:
        tier = 2
      elif "t01" in survivor_id:
        tier = 1

      if "_ur_" in survivor_id:
        reality = "mythic"
      elif "_sr_" in survivor_id:
        reality = "legendary"
        for index, mythic_lead_id in enumerate(mythic_lead_list):
          if mythic_lead_id in survivor_id:
            reality = "mythic"
            break
      elif "_vr_" in survivor_id:
        reality = "epic"
      elif "_r_" in survivor_id:
        reality = "rare"
      elif "_uc_" in survivor_id:
        reality = "uncommon"
      elif "_c_" in survivor_id:
        reality = "common"

      if"manager" in templateId:
        for lead_squad in lead_much:
          if lead_squad in templateId:
            lead_in_squad = squad_mapping[lead_squad]

      # survivor_squadにサバイバーと情報を配置していく
      if squad_slot_idx == 0:
        survivor_squad[squad_id]["leader"] = {"lead_in_squad": lead_in_squad, "personality": personality, "level": level, "tier": tier, "reality": reality}
      else:
        survivor_squad[squad_id]["teams"][int(squad_slot_idx -1)] = {"personality": personality, "level": level, "tier": tier, "reality": reality}

# 配置したサバイバーの情報や関係性から評価し、スクワッドに対応しているF.O.R.T.値に加算していく。
for squad_name, squad in survivor_squad.items():
  squad_points = 0
  leader = squad["leader"]

  if leader is None:
    leader_points = 0
    lead_reality = None
    lead_level = None
    lead_tier = None
    lead_personality = None
    lead_in_squad = None
  else:
    lead_reality = leader["reality"]
    lead_level = leader["level"]
    lead_tier = leader["tier"]
    lead_personality = leader["personality"]

    lead_in_squad = leader["lead_in_squad"]
    if lead_in_squad == squad_name:
      leader_bonus = 2
    else:
      leader_bonus = 1
  if not leader is None:
    leader_points = (round( base_power_mapping[lead_reality] + ( (lead_level - 1) * float(level_constant_mapping["lead_survivors"][lead_reality]) ) + ( (lead_tier - 1) * float(evolution_constant_mapping["lead_survivors"][lead_reality]) ) )) *leader_bonus

  teams = squad["teams"]
  for member in teams:
    if member == None:
      continue

    member_reality = member["reality"]
    member_level = member["level"]
    member_tier = member["tier"]
    member_personality = member["personality"]

    if member_personality == lead_personality:
      if lead_reality == "mythic":
        team_bonus = 8
      elif lead_reality == "legendary":
        team_bonus = 5
      elif lead_reality == "epic":
        team_bonus = 4
    elif lead_personality == None:
      team_bonus = 0
    else:
      if lead_reality == "mythic":
        team_bonus = -2
      elif lead_reality == "legendary":
        team_bonus = 0
      elif lead_reality == "epic":
        team_bonus = 0

    member_points = int(Decimal(str( base_power_mapping[member_reality] + ( (member_level - 1) * float(level_constant_mapping["survivor"][member_reality]) ) + ( (member_tier - 1) * float(evolution_constant_mapping["survivor"][member_reality]) ) )).quantize(Decimal('1'), rounding=ROUND_HALF_UP) +team_bonus)
    squad_points = squad_points + member_points

  if lead_in_squad == "squad_attribute_medicine_emtsquad" or lead_in_squad == "squad_attribute_medicine_trainingteam":
    fort_mapping = "fortitude"
  elif lead_in_squad == "squad_attribute_arms_closeassaultsquad" or lead_in_squad == "squad_attribute_arms_fireteamalpha":
    fort_mapping = "offense"
  elif lead_in_squad == "squad_attribute_scavenging_scoutingparty" or lead_in_squad == "squad_attribute_scavenging_gadgeteers":
    fort_mapping = "resistance"
  elif lead_in_squad == "squad_attribute_synthesis_corpsofengineering" or lead_in_squad == "squad_attribute_synthesis_thethinktank":
    fort_mapping = "technology"

  fort[fort_mapping] = fort[fort_mapping] + squad_points + leader_points
```

以上2つのコードで加算されていった最終的なfortがF.O.R.T.値になります。
