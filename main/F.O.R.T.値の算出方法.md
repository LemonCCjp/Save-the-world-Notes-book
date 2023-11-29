# F.O.R.T.値の算出方法

QueryProfileに載っていない、アカウントのF.O.R.T.値を算出する方法。

## 1. リサーチ。コマンダーレベル。サバイバースクワッド。の3つの情報を取得して対応・計算する。

まず、リサーチレベルを取得する必要があります。

以下のコードでリサーチレベルから各F.O.R.T.値に対応させます。

(dataは取得したQueryProfileです。)

```python
#　クソコードでごめんなさい

# リサーチレベルごとの得られるF.O.R.T.値の辞書
research_fort = {'1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 17, '11': 18, '12': 19, '13': 20, '14': 21, '15': 22, '16': 23, '17': 24, '18': 25, '19': 26, '20': 38, '21': 39, '22': 40, '23': 41, '24': 42, '25': 43, '26': 44, '27': 45, '28': 46, '29': 47, '30': 65, '31': 66, '32': 67, '33': 68, '34': 69, '35': 70, '36': 71, '37': 72, '38': 73, '39': 74, '40': 100, '41': 101, '42': 102, '43': 103, '44': 104, '45': 105, '46': 106, '47': 107, '48': 108, '49': 109, '50': 143, '51': 144, '52': 145, '53': 146, '54': 147, '55': 148, '56': 149, '57': 150, '58': 151, '59': 152, '60': 192, '61': 193, '62': 194, '63': 195, '64': 196, '65': 197, '66': 198, '67': 199, '68': 200, '69': 201, '70': 247, '71': 248, '72': 249, '73': 250, '74': 251, '75': 252, '76': 253, '77': 254, '78': 255, '79': 256, '80': 308, '81': 309, '82': 310, '83': 311, '84': 312, '85': 313, '86': 314, '87': 315, '88': 316, '89': 317, '90': 375, '91': 376, '92': 377, '93': 378, '94': 379, '95': 380, '96': 381, '97': 382, '98': 383, '99': 384, '100': 446, '101': 447, '102': 448, '103': 449, '104': 450, '105': 451, '106': 452, '107': 453, '108': 454, '109': 455, '110': 521, '111': 522, '112': 523, '113': 524, '114': 525, '115': 526, '116': 527, '117': 528, '118': 529, '119': 530, '120': 600}

# ここでリサーチレベルを取得します。　
research_levels = data["profileChanges"][0]["profile"]["stats"]["attributes"]["research_levels"]

# F.O.R.T.値の初期化
fort = {"fortitude": 0, "offense": 0, "resistance": 0, "technology": 0}

# 取得したレベルに対応したF.O.R.T.値がfortに加算されます。
for k, v in research_levels.items():
  fort[k] += research_fort[str(v)]

```

次に、コマンダーレベルに応じたF.O.R.T.値を対応させてfortに加算します。

```python
# こっちもめっちゃコード汚いです。

# コマンダーレベルごとの得られるF.O.R.T.値の辞書
commander_level_fort = {"2": {"fortitude": 3, "offense": 0, "resistance": 0, "technology": 0}, "3": {"fortitude": 3, "offense": 3, "resistance": 0, "technology": 0}, "4": {"fortitude": 3, "offense": 3, "resistance": 3, "technology": 0}, "5": {"fortitude": 3, "offense": 3, "resistance": 3, "technology": 3}, "11": {"fortitude": 6, "offense": 3, "resistance": 3, "technology": 3}, "12": {"fortitude": 6, "offense": 6, "resistance": 3, "technology": 3}, "13": {"fortitude": 6, "offense": 6, "resistance": 6, "technology": 3}, "14": {"fortitude": 6, "offense": 6, "resistance": 6, "technology": 6}, "21": {"fortitude": 9, "offense": 6, "resistance": 6, "technology": 6}, "22": {"fortitude": 9, "offense": 9, "resistance": 6, "technology": 6}, "23": {"fortitude": 9, "offense": 9, "resistance": 9, "technology": 6}, "24": {"fortitude": 9, "offense": 9, "resistance": 9, "technology": 9}, "31": {"fortitude": 13, "offense": 9, "resistance": 9, "technology": 9}, "32": {"fortitude": 13, "offense": 13, "resistance": 9, "technology": 9}, "33": {"fortitude": 13, "offense": 13, "resistance": 13, "technology": 9}, "34": {"fortitude": 13, "offense": 13, "resistance": 13, "technology": 13}, "41": {"fortitude": 17, "offense": 13, "resistance": 13, "technology": 13}, "42": {"fortitude": 17, "offense": 17, "resistance": 13, "technology": 13}, "43": {"fortitude": 17, "offense": 17, "resistance": 17, "technology": 13}, "44": {"fortitude": 17, "offense": 17, "resistance": 17, "technology": 17}, "56": {"fortitude": 21, "offense": 17, "resistance": 17, "technology": 17}, "57": {"fortitude": 21, "offense": 21, "resistance": 17, "technology": 17}, "58": {"fortitude": 21, "offense": 21, "resistance": 21, "technology": 17}, "59": {"fortitude": 21, "offense": 21, "resistance": 21, "technology": 21}, "61": {"fortitude": 26, "offense": 21, "resistance": 21, "technology": 21}, "62": {"fortitude": 26, "offense": 26, "resistance": 21, "technology": 21}, "63": {"fortitude": 26, "offense": 26, "resistance": 26, "technology": 21}, "64": {"fortitude": 26, "offense": 26, "resistance": 26, "technology": 26}, "71": {"fortitude": 31, "offense": 26, "resistance": 26, "technology": 26}, "72": {"fortitude": 31, "offense": 31, "resistance": 26, "technology": 26}, "73": {"fortitude": 31, "offense": 31, "resistance": 31, "technology": 26}, "74": {"fortitude": 31, "offense": 31, "resistance": 31, "technology": 31}, "86": {"fortitude": 37, "offense": 31, "resistance": 31, "technology": 31}, "87": {"fortitude": 37, "offense": 37, "resistance": 31, "technology": 31}, "88": {"fortitude": 37, "offense": 37, "resistance": 37, "technology": 31}, "89": {"fortitude": 37, "offense": 37, "resistance": 37, "technology": 37}, "96": {"fortitude": 43, "offense": 37, "resistance": 37, "technology": 37}, "97": {"fortitude": 43, "offense": 43, "resistance": 37, "technology": 37}, "98": {"fortitude": 43, "offense": 43, "resistance": 43, "technology": 37}, "99": {"fortitude": 43, "offense": 43, "resistance": 43, "technology": 43}, "106": {"fortitude": 50, "offense": 43, "resistance": 43, "technology": 43}, "107": {"fortitude": 50, "offense": 50, "resistance": 43, "technology": 43}, "108": {"fortitude": 50, "offense": 50, "resistance": 50, "technology": 43}, "109": {"fortitude": 50, "offense": 50, "resistance": 50, "technology": 50}, "121": {"fortitude": 57, "offense": 50, "resistance": 50, "technology": 50}, "122": {"fortitude": 57, "offense": 57, "resistance": 50, "technology": 50}, "123": {"fortitude": 57, "offense": 57, "resistance": 57, "technology": 50}, "124": {"fortitude": 57, "offense": 57, "resistance": 57, "technology": 57}, "131": {"fortitude": 118, "offense": 108, "resistance": 108, "technology": 108}, "132": {"fortitude": 118, "offense": 118, "resistance": 108, "technology": 108}, "133": {"fortitude": 118, "offense": 118, "resistance": 118, "technology": 108}, "134": {"fortitude": 118, "offense": 118, "resistance": 118, "technology": 118}, "141": {"fortitude": 72, "offense": 64, "resistance": 64, "technology": 64}, "142": {"fortitude": 72, "offense": 72, "resistance": 64, "technology": 64}, "143": {"fortitude": 72, "offense": 72, "resistance": 72, "technology": 64}, "144": {"fortitude": 72, "offense": 72, "resistance": 72, "technology": 72}, "156": {"fortitude": 80, "offense": 72, "resistance": 72, "technology": 72}, "157": {"fortitude": 80, "offense": 80, "resistance": 72, "technology": 72}, "158": {"fortitude": 80, "offense": 80, "resistance": 80, "technology": 72}, "159": {"fortitude": 80, "offense": 80, "resistance": 80, "technology": 80}, "176": {"fortitude": 89, "offense": 80, "resistance": 80, "technology": 80}, "177": {"fortitude": 89, "offense": 89, "resistance": 80, "technology": 80}, "178": {"fortitude": 89, "offense": 89, "resistance": 89, "technology": 80}, "179": {"fortitude": 89, "offense": 89, "resistance": 89, "technology": 89}, "196": {"fortitude": 98, "offense": 89, "resistance": 89, "technology": 89}, "197": {"fortitude": 98, "offense": 98, "resistance": 89, "technology": 89}, "198": {"fortitude": 98, "offense": 98, "resistance": 98, "technology": 89}, "199": {"fortitude": 98, "offense": 98, "resistance": 98, "technology": 98}, "221": {"fortitude": 108, "offense": 98, "resistance": 98, "technology": 98}, "222": {"fortitude": 108, "offense": 108, "resistance": 98, "technology": 98}, "223": {"fortitude": 108, "offense": 108, "resistance": 108, "technology": 98}, "224": {"fortitude": 108, "offense": 108, "resistance": 108, "technology": 108}, "241": {"fortitude": 128, "offense": 118, "resistance": 118, "technology": 118}, "242": {"fortitude": 128, "offense": 128, "resistance": 118, "technology": 118}, "243": {"fortitude": 128, "offense": 128, "resistance": 128, "technology": 118}, "244": {"fortitude": 128, "offense": 128, "resistance": 128, "technology": 128}, "256": {"fortitude": 139, "offense": 128, "resistance": 128, "technology": 128}, "257": {"fortitude": 139, "offense": 139, "resistance": 128, "technology": 128}, "258": {"fortitude": 139, "offense": 139, "resistance": 139, "technology": 128}, "259": {"fortitude": 139, "offense": 139, "resistance": 139, "technology": 139}, "271": {"fortitude": 151, "offense": 139, "resistance": 139, "technology": 139}, "272": {"fortitude": 151, "offense": 151, "resistance": 139, "technology": 139}, "273": {"fortitude": 151, "offense": 151, "resistance": 151, "technology": 139}, "274": {"fortitude": 151, "offense": 151, "resistance": 151, "technology": 151}, "291": {"fortitude": 164, "offense": 151, "resistance": 151, "technology": 151}, "292": {"fortitude": 164, "offense": 164, "resistance": 151, "technology": 151}, "293": {"fortitude": 164, "offense": 164, "resistance": 164, "technology": 151}, "294": {"fortitude": 164, "offense": 164, "resistance": 164, "technology": 164}}

#　ここでコマンダーレベルを取得します。
commander_level = int(data["profileChanges"][0]["profile"]["stats"]["attributes"]["level"])

# その時点のレベルで何レベル～何レベルまでの間に当てはまるかを調べて、当てはまったところのF.O.R.T.値をfortに加算する。
last_level = 0
for tmp_cl, l_for_fort in commander_level_fort.items():
  key_list = list(commander_level_fort.keys())
  index = int(key_list.index(tmp_cl))
  if index == 87:
    next_level = 295
  else:
    next_level = int(key_list[index +1])
  
  if commander_level >= 295:
    clr = commander_level_fort["294"]
    break
  elif tmp_cl == commander_level:
   clr =  l_for_fort
   break
  elif last_level < commander_level < next_level:
    clr =  commander_level_fort[str(last_level)]
    break
  else:
    last_level = int(tmp_cl)
  
for forty, value in clr.items():
  fort[forty] = fort[forty] +value

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

以上3つのコードで加算されていった最終的なfortがF.O.R.T.値になります。
