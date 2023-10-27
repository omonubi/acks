# Roll20 Character Sheet for Adventurer Conqueror King System, by Autarch
This is a Roll20 specific character sheet designed for use with the Adventurer Conqueror King System (ACKS) OSR RGP, by Autarch.

Hopefully, this documentation below comes in useful. I know I've struggled over the years when attempting to implement custom character sheets. Even the official sheets are sometimes difficult to divine.

A note about automation: There's a wide range of freedom in how a sheet developer can automate and/or embed a game's mechanics into their Roll20 custom sheet. While more automation can make it easier for the players, it often make the sheet more complex to maintain, increases sheet loading times, slows performance, and (usually) restricts flexibility. For this sheet, automation and embedded game mechanics, tables, etc., has been limited as much as possible. A good example is the Hiring tab > Mercenaries. A lot of this could have been automated with drop-downs containing embedded mercenary information from the ACKS rulebook, but the sheet worker complexity, combined with reduction in flexibility of using these tools, led me not to do so.

---

## Version 3.0
- This is v3 of the character sheet, for supporting ACKS II.
- With ACKS II in beta, the current rules version refereced is v109.
- Many small changes to this sheet were needed.
- Several optional rules references not specifically included in ACKS II were removed from the sheet. E.G., base healing rate.
- Some changes made to how attacks are executed on the Combat tab.
- The Hirelings tab was overhauled to include loyalty and obedience checks.

---

## How To...

### How to Install the Character Sheet
1. In Roll 20, click *Create New Game* and select *Custom* from the *Optional: Choose Character Sheet* drop-down menu.
2. Once the game is created, go to *Settings* > *Game Settings*.
3. Under *Character Sheet Template*...
    1. Within the *HTML Layout* tab, paste in the contents of charactersheet.html.
    2. Within the *CSS Styiling* tab, paste in the contents of charactersheet.css.
    3. Ensure that *Legacy Sanitation* is **NOT** checked.
4. Save your changes and launch the game.

**This sheet does not yet support translation.**

### How to Use the Character Sheet
1. Generate an ACKS character.
2. On the **Overview** tab, enter the creature's basic info and its generated attributes. Save each attribute's Nominal value in the Max column of the Roll20 sheet's Attributes & Abilities tab. Generate the character's hit points and save the value in both Hit Point fields.
3. On the **Class** tab, input any class-specific modifiers and a desired class description. If the creature is a spellcaster, enter the maximum and current spell slots by spell level.
4. On the **Skills** tab, enter the creature's selected proficiency information and any additional ability information desired. (Note: Use the **!AddSkills** custom API script to pre-populate common abiltiies and proficiencies - see later in this document.)
5. Enter the appropriate information on the **Combat** tab, based on the creature's equipment and other characteristics. Enter the creature's melee and missile attack options. Use one line-item for each attack type, combination, and/or damage formula. Check the (equipped) checkboxes to indicate which attacks are available at any given time.
7. On the **Equipment** tab, save the creature's current equipment, currency info, and monthly budget. All of this information is optional for NPCs and monsters.
8. Use the **Hirelings** tab to keep track of Henchmen, Mercenaries, and Specialists in the creature's employ.
9. Use the **Journal** tab to capture additional information such as background, appearance, languages spoken, injuries, and property. The Adventure Notes can be used by the player to keep track of miscellaneous details. The GM uses the Experience Log to input earned experience after the creature completes an expedition.
10. The GM can use the **Settings** tab to customize or modify certain core base traits used in sheet calculations, such as bas movement rate, encumbrance thresholds, and earned XP modifiers.
11. Use the **whisper toggle** to send any Roll20 Chat window output as a whisper to oneself. Otherwise, any such Chat output is viewable by all.

**Refer to the Sheet Variable Manifest, below, for details on how each field and control function.**

### How to Use the Ability and Proficiency Target Fields
- By default the each new Ability and Proficiency item's Target field value is empty.
- If the Target field value is empty and the Throw button is clicked, nothing will happen. This is by design.
- Any Roll20 inline roll formula (including roll queries) can be used to set the Target value of an Ability or Proficiency list item. Use the Variable Manifest, below, to find the names of specific sheet variables. Examples include:
    - 18
    - 18-4[elf]
    - 18-@{str_mod}*4
    - ?{Target|Normal,14|Forest,3}
    - 18+(@{has_helm}[helm]*4)
    - Etc.

### How to Use Melee and Missile Attack Lists
1. Use the Melee Attacks list for melee attacks, and the Missile Attacks list for missile  and/or thrown attacks. Do not mix these.
2. For each possible attack, attack combination, and/or damage formula, create a separate attack entry. For example, a spear could have up to 5 entries; 4 melee and 1 missile:
    1. Melee: "Spear (One-Handed)", 1d6 damage
    2. Melee: "Spear (Two-Handed)", 1d8 damage
    3. Melee: "Spear (Broken, Tip), -1 bonus, 1d4 damage
    4. Melee: "Spear (Broken, Haft), -1 bonus, 1d4 damage
    5. Missile: "Spear (Thrown), isThrown, 1d6 damage
3. If the creature is proficient in the dual wield fighting style, creating entries for each weapon combination likely to be used:
    1. "Mace +1 and Sword", +2 bonus, 1d6+1 damage
    2. "Sword and Dagger", +1 bonus, 1d6 damage
    3. Etc.
3. The Melee Attacks > Long checkbox and the Missile Attacks > Range field are optional but provide helpful context to the player.
4. By design, Missile Attacks do **not** have a field for tracking ammunition. It is suggested to track creature ammunition on the Equipment tab or some other way.
5. Note: Use the **!AddActions** custom API script to automagically include common attacks to both Melee and Missile Attack lists. These entires can then be customized as needed for both monsters and characters.

### How to Work with Equipment and Money
- Uncheck the Equipped checkbox to "warehouse" (not carry) inventory.
- If tracking amunition on the Equipment tab, each round weighs 0.02 stone.
- If tracking individual items within a "small item" (such as 12 spikes), track the count within the item's name. Alternately, reduce the Weight of each item by the appropriate amount and use the Count field.
- There are multiple ways to handle rations (assumed to be iron):
    - Individually, each day's ration weighs 1/6 (0.17) stone. 
    - Each day's waterskin (full) weighs 5/6 (0.83) stone.
    - Combined, a "day's rations" (food and water) weigh 1 stone.
    - If using the ACKS II survival simplified approach, a "day's rations" weigh 0.5 stone.
- Each coin weighs 0.001 stone. Use the Banked column to offload coinage On Hand. Use the New column to add or remove coins from the inventory. The GM should use this feature, then ask the player to click the Claim button and move it to On Hand. 

### How to Award and Track Experience Points
- The Overview > XP and Journal > Experience Log fields are designed to track and award XP in discrete chunks, based on the ACKS "return to civilization" approach.
- The Journal > Experience Log repeating list *must* be used to add XP to the character's XP total.
- The creature's total XP is updated to reflect changes in the Experience Log whenever an entry is added, changed, or removed.
- When adding a log entry, reference the creature's prime requisite XP bonus and enter than into the log entry. (XP% bonus can change for the creature over the course of play, such as brain injury.)

## Sheet Variable Manifest
This section details all fields and controls on the sheet and how they are calculated, used, etc.

1. **IMPORTANT:** Most buttons on the sheet *require* that a token is selected in order to function.
2. The term 'creature' refers to any player character, NPC, or monster who this sheet is used to represent. When representing monsters, many of the fields described can be safely ignored.
3. All field values are optional, unless otherwise specified.
4. Attribute variable names shown below do not include the *attr_* portion of the name. The names for roll buttons and repeating sections include the full variable name.
5. All *CAPS* text within a *variable_name* indicates a placeholder for that indicated value. E.G., '*some_LVL_variable*' indicates a level number for the *LVL* portion of the variable name.
6. Some fields are included to hold "reference values". These are purely informational fields that aren't used any place else on the sheet, and are identified as such, below.
7. Some sections of the character sheet employ checkbox toggles used to show/hide sections of content or details specific to the item type in question. These toggle checkboxes are not always labeled, but should be self-evident when clicked.

---

### The Overview Tab
This tab contains all the general information for the creature, including a summary of important key values used during play.

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| Character Name | *character_fullname* | text | Provided to allow for more 'expressive' and complete names than either the standard *character_name* or *token_name* properties. |
| Token Name | *character_name* | text | The name used for both the sheet and the token. |
| Class | *class* | text | The character's class. |
| Gender | *gender* | text | The character's gender. |
| Title | *character_title* | text | The character's title. |
| Height | *height* | text | Ther character's height. |
| Alignment | *alignment* | text | The character's alignment. |
| Weight | *weight* | text | The character's weight. |
| Experience | *xp_bonus* | calculated | A percentage auto-calculated based on the character's prime requisite(s) attribute scores. Can be modified in Settings. Is **not** automatically applied to XP calculations (see Journal tab). |
| Experience | *xp* | calculated | The character's total experience points. Calculated by totalling Experience Log entries on the Journal tab. |
| Experience | *xp_next_level* | text | The amount of experience points needed by the character for next level. |
| Age | *age* | number | The character's current age. |
| Level | *level* | number | The character's current level. The minimum allowed is 0; the maximum is 14. Note: The sheet doesn't limit level based on class. |
| Hit Dice | *hit_dice* | text | The character's current number of hit dice, in Roll20 /roll format. |
| Hit Points | *roll_hit_dice* | button | Rolls the Hit Dice formula above in the Roll20 Chat window. |
|||||
| Attributes, Current | *str*, *int*, *wis*, *dex*, *con*, *chr*  | number | Minimum value for each is 0; maximum is 20. |
| Attributes, Nominal | *VAR_max* | number | Intended to preserve the character's un-modified score for each attribute during play, for reference. Values are set using the sheet's Roll20 Attributes & Abilities tab. |
| Attributes, Mod | *VAR_mod* | calculated | Each attribute's auto-calculated modifier. |
|||||
| Hit Points | *hp* | number | The character's current amount of hit points. |
| HP Max | *hp_max* | number | The character's maximum amount of hit points. |
| Armor Class | *armor_class* | duplicate | See the Combat tab. |
| Attack Throw | *attack_throw* | duplicate | See the Combat tab. |
| Encumbrance | *encumbrance* | duplicate | See the Equipment tab. |
| Capacity, Max | *enc_capacity* | duplicate | See the Equipment tab. |
| Feet / Turn | *move_turn* | duplicate | See the Equipment tab. |
| Feet / Round | *move_round* | duplicate | See the Equipment tab. |
| Miles / Day | *miles_day* | calculated | *move_turn* / 5. |
| Miles / Hour | *miles_hour* | calculated | *move_turn* / 40. |
| Fatigue Level | *fatigue* | number | The character's current fatigue level. Modifiers from fatigue are automatically applied to sheet stats and rolls. |
| Needs Bedrest (Days) | *bedrest* | number | The amount of bedrest the character currently needs before they can heal. Purely informational. |
|||||
| Saving Throws | *save_1*, *save_2*, *save_3*, *save_4*, *save_5* | number | The character's base saving throw targets. |
|||||
| Alt Move | *alt_move_mode* | text | The creature's alternative form of movement, if any. |
| Feet / Round | *alt_move* | text | The speed of the creature's alternative form of movement, if any. |
| Morale | *morale* | number | The creature's morale score (default is 0). |
| XP Value | *xp_value* | number | The creature's value in experience points. |
| Save | *monster_save* | text | A letter and a number (E.G. 'F1') that indicates a creature's saving throw progression found in their creature description. Valid letters are 'F', 'C', 'M', 'T', 'D', and 'E'. Numbers can range from 0 to 14. Setting to a valid string value auto-computes and overwrites the creature's base saving throws, above. | 
| Treasure Type | *monster_treasuretype* | text | The creature's treasure type, if any. |
| Size | *monster_size* | list | The monster's size. The default is 'medium' (man-sized). |

---

### The Class Tab
This tab keeps track of class-specific information, such as global modifiers, class description, and spellcasting information.

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| Accuracy | *accuracy_bonus* | number | Applied to all missile weapon attack throws. |
| Dmg, Melee | *damage_bonus* | number | Applied to all melee weapon damage rolls. |
| Dmg, Missile | *damage_bonus_missile* | number | Applied to all missile weapon damage rolls. |
| Dmg Reduction | *damage_reduction* | number | Tracks the creature's global incoming damage reduction modifier, if any. Is **not** applied automatically. |
| Armor Class | *class_ac_bonus* | Number | Applied to all armor class calculations. |
| Initiative | *class_initiative_bonus* | Number | Applied to all initiaitve rolls. |
| Surprise | *class_surprise_bonus* | Number | Applied to all surprise rolls. |
| Hench Morale | *class_henchmen_morale_bonus* | Applied to all henchmen morale checks. |
|||||
| Class Abilities | *class_abilities* | text | A text area intended for displaying all class-specific details (except tabular data) for the creature's class. I.e., copying in an ACKS class description. |
|||||
| Spellcasting (Slots) | *spells_LVL_max* | number | The maximum number of spells available, by spell level. Min is 0; max is 6. |
| Spellcasting (Current) | *spells_LVL* | number | The current number of spells the creature has remaining, by spell level. Min is 0; max is 6. |
| Caster Level | *caster_level* | number | The character's caster level. Min is 0; max is 14. |
|||||
| **Spells** | *repeating_spells_arcane* | repeating | A list of the creature's spells, if any. |
| Cast | *roll_cast_spell* | button | When clicked, exports the spell's information (below) to the Roll20 Chat window. Note that [[in-line rolls]] can be included in the texts below, if desired. |
| (name) | *spell_name* | text | The name of the spell. |
| Level | *spell_level* | number | The spell's level. Can be set to 1 - 9, with a default vaule of 1. |
| Range | *spell_range* | text | The spell's range description. |
| Duration | *spell_duration* | text | The spell's duration description. |
| Prepared | *spell_prepared* | checkbox | Whether the spell is currently prepared or not. Only prepared spells are listed if/when the Action macro is used (see below). |
| Description | *spell_description* | text | The spell's text description. |

---

### The Skills Tab
This tab is used to keep track of proficiencies and derivative / supporting ability information. Many common, starting abilities and proficiencies can be automagically added using the !AddSkills script, detailed later in this document.

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| **Abilities** | *repeating_abilities* | repeating | A list of the creature's non-proficiency abilities. Can be used to add specific proficiency-driven abilities such as *Listen* (Adventuring) or *Diagnose Illness* (Healing), class-specific ability descriptions, or anything else desired. |
| Throw | *roll_ability_check* | button | When clicked, makes a 1d20 ability throw against the Target value. If no Target is specified, the throw does not occur. Includes modifiers from fatigue and roll-specific (queried) conditions.  |
| (name) | *ability_name* | text | The name of the ability. |
| Source | *ability_source* | text | The source of the ability, for reference purposes. This is usually the name of a proficiency, but doesn't have to be. |
| Target | *ability_target* | text | Empty by default. If present, should be a formula used to set the ability's throw target (see below). |
| OnCD | *ability_cooldown* | checkbox | Whether the ability is currently on cooldown.
| Description | *ability_details* | text | The ability's text description. |
|||||
| **Proficiencies** | *repeating_skills* | repeating | A list of the creature's proficiencies. |
| Throw | *roll_skill_check* | button | When clicked, makes a 1d20 proficiency throw against the Target valie. If no Target is specified, the throw does not occur. Includes modifiers from fatigue and roll-specific (queried) conditions. |
| (name) | *skill_name* | text | The name of the proficiency. |
| Rank | *skill_rank* | number | The creature's rank in the proficiency. Minimum value is '1'; maximum is '4'; default is '1'. Rank is automatically factored into the throw Target, if any. |
| Type | *skill_type* | list | The type of proficiency: *General*, *Class*, or *Class Power*. The default is *General*. |
| Target | *skill_target* | text | Empty by default. If present, should be a formula used to set the proficiency's throw target (see below). |
| Description | *skill_details* | text | The proficiency's text description. |
---

### The Combat Tab
This tab contains all combat-specific data, including encounter-related rolls, armor class and attack information, melee attacks, and missile attacks.

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| Initiative | *roll_initiative* | button | When clicked, rolls 1d6 for the creature's initiative. Includes modifiers from DEX modifier, class bonus, and temporary (see below) and roll-specific (queried) conditions. |
| Initiative | *mod_initiative* | number | Used to apply a temporary but persistent modifier to initiative rolls. |
| Surprise | *roll_surprise* | button | When clicked, rolls the creature's 2d6 surprise check. Includes modifiers from class bonus, heavy helms, and temporary (see below) and roll-specific (queried) conditions. |
| Surprise | *mod_surprise* | number | Used to apply a temporary but persistent modifier to surprise rolls. |
| Reaction | *roll_reaction* | button | When clicked, rolls a 2d6 reaction check to the creature. Includes modifiers from CHA modifier, and temporary (see below) and roll-specific (queried) conditions. |
| Reaction | *mod_reaction* | number | Used to apply a temporary but persistent modifier to reaction rolls. |
| Morale | *roll_morale* | button | When clicked, rolls a 2d6 morale check for the creature. Includes the creature's morale modifier (if any), and temporary (see below) and roll-specific (queried) conditions. Generally used only for NPCs and monsters. |
| Morale | *mod_morale* | number | Used to apply a temporary but persistent modifier to morale rolls. |
|||||
| Armor | *armor* | list | Used to select a creature's currently worn armor. The default is *AC 0 - Clothing Only*. |
| Bonus | *armor_bonus* | number | Used to set an additional armor bonus. For example, penalties from cursed armor or bonuses from magical armor. The default is 0. Note: To set a creature's natural AC, use the Class tab > Armor Bonus field. |
| Shield | *has_shield* | checkbox | Indicates whether the creature is currently using a shield. |
| Helm | *has_helm* | checkbox | Indicates whether the creature is currently wearing a **heavy** helm. |
| Armor Class | *armor_class* | Calculated | The creature's current armor class. Calculated as (*armor* AC value) + (*armor* AC modifier) + (1, if shield) + (class AC bonus) + (*dex_mod*).
| Fighting Style Specialization | *fss* | list | The creature's current fighting style specialization, if any. Used purely for sheet reference - does not impact other fields or rolls. For more info, see the proficiency description. |
|||||
| Attack Throw | *attack_throw* | number | The creature's base attack throw. The minimum value is -9; maximum is 12; default is 10. |
| Max Cleaves | *max_cleaves* | number | The creature's number of maximum cleaves based on class and level. Other constraints are not include. The minimum value is 0; maximum is 14; default is 0. |
| Number of Attacks | *num_attacks* | text | The creature's attack routine, used purely for reference. The default is "*1 (weapon)*".
|||||
| **Melee Attacks** | *repeating_melee_attacks* | repeating | A list of the creature's melee attacks. |
| Attack | *roll_melee_attack* | button | When clicked, makes a 1d20 attack throw for the melee attack. The target value is set to the creature's *attack_throw* value - the target's AC (queried). Modifiers to the roll include the creature's STR modifier, the attack's melee attack modifier, fatigue, and a roll-specific (queried) modifer. Both natural 1s and 20s are emphasized in the roll results output. Additionally, the attack's damage is rolled as well (see below) and included in the results, whether or not the throw is successful. |
| (equipped) | *melee_equip* | checkbox | When checked, indicates that the attack is currently equipped (if a weapon) and the attack is available. Note that multiple attacks (E.G. a d6 sword attack and a d8 sword attack) may be listed as seperate attacks and equppied simultaneously. This checkbox is used to determine which attacks are displayed on the creature's Actions macro (see below). |
| (name) | *melee_name* | text | The name of the melee attack. |
| Bonus | *melee_bonus* | number | An attack-specific modifier applied to the weapon's attack throw. E.G., a magical or cursed weapon's modifier. This modifier is **not** applied to the attack's damage roll. |
| Dmg | *roll_melee_damage* | button | When clicked, rolls the attack's Damage formula (see below). The roll is modified by the creature's STR modifier, its class damage bonus (if any), and fatigue effects. The total rolled can never be less than 1. |
| (damage formula) | *melee_damage* | text | This is an inline roll formula expressing the attack's damage dice and modifier. E.G., "1d6+1". | 
| Long | *melee_long* | checkbox | Indicates if the attack has the *Long* characteristic (has reach). This value is purely for reference. |
| Effects | *melee_effects* | text | An optional field for describing any additional effects related to the attack, such as poison, paralysis, visual description, etc. This is output to the Roll20 Chat when the Attack button is clicked. |
|||||
| **Missile Attacks** | *repeating_missile_attacks* | repeating | A list of the creature's missile attacks. |
| Attack | *roll_missile_attack* | button | When clicked, makes a 1d20 attack throw for the missile attack. The target value is set to the creature's *attack_throw* value - the target's AC (queried). Modifiers to the roll include the creature's DEX modifier, the attack's missile attack modifier, a range modifier (queried), a class accuracy bonus (if any), fatigue, and a roll-specific (queried) modifer. Both natural 1s and 20s are emphasized in the roll results output. Additionally, the attack's damage is rolled as well (see below) and included in the results, whether or not the throw is successful. |
| (equipped) | *missile_equip* | checkbox | When checked, indicates that the attack is currently equipped (if a weapon) and the attack is available. Note that multiple attacks (E.G. a d6 arrow attack and a d6+1 magic arrow attack) may be listed as seperate attacks and equppied simultaneously. This checkbox is used to determine which attacks are displayed on the creature's Actions macro (see below). |
| (name) | *missile_name* | text | The name of the missile attack. |
| Bonus | *missile_bonus* | number | An attack-specific modifier applied to the weapon's attack throw. E.G., a magical or cursed weapon's modifier. This modifier is **not** applied to the attack's damage roll. |
| Dmg | *roll_missile_damage* | button | When clicked, rolls the attack's Damage formula (see below). The roll is modified by the creature's STR modifier (if Thrown - see below), its class damage bonus (if any), and fatigue effects. The total rolled can never be less than 1. |
| (damage formula) | *missile_damage* | text | This is an inline roll formula expressing the attack's damage dice and modifier. E.G., "1d6+1". | 
| Range | *missile_range* | text | Used to describe the missile attack's range bands. E.G, "15/30/45". This is used purely for reference. |
| Thrown | *missile_has_str_bonus* | checkbox | If checked, applies the creature's STR modifier to the attack's damage roll. |
| Effects | *missile_effects* | text | An optional field for describing any additional effects related to the attack, such as poison, paralysis, visual description, etc. This is output to the Roll20 Chat when the Attack button is clicked. |

---

### The Equipment Tab
This tab keeps track of all equipment and coinage. It also has optional controls for tracking standard of living and monthly expenses.

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| Max, Capacity | *enc_capacity* | calculated | This is the creature's maximum carrying capacity. It is calculated as the *base_enc* (see Settings tab) + the creature's STR modifier. |
| Encumbrance | *encumbrance* | calculated | This is the creature's current encumbrance total, rounded up. Encumbrance is re-calculated each time an item's Weight or Count is changed, and each time Currency On Hand changes. |
| Feet / Turn | *move_turn* | calculated | This is the creature's current movement rate per game turn. It is calculated by comparing the creature's current encumbrance against the encumbrance thresholds defined on the Settings tab. By default, these thresholds are set to those for humanoid, man-sized creatures, as per ACKS. |
| Feet / Round | *move_round* | calculated | This is the creature's current movement rate per game round. It is computed as *move_turn* / 3. |
|||||
| **Equipment** | *repeating_items* | repeating | A list of the creature's equipment / inventory, both carried or otherwise. |
| (name) | *item_name* | text | The name of the equipment item. |
| # | *item_count* | number | A count of the number of items included. The minimum value is 0. There is no maximum. The default value is 1. |
| Wt | *item_weight* | number | The item's weight, in stone. The minimum value is 0. There is no maximum. The default weight for all new items in 0.17 (one-sixth of a stone, as per ACKS encumbrance for small items). |
| Value | *item_value* | number | An optional field for tracking the value of the item, for reference only. By default, this field is empty. Typically, used only for tracking the value of valuable and/or exceptional items. |
| Equipped | *item_equipped* | checkbox | If checked, the item's total *item_weight* x *item_count* is included in the creature's encumbrance. Otherwise, it is not. The default value is *checked*. |
| Details | *item_details* | text | An optional text area to add item details, for reference only. |
|||||
| Currency, On Hand | *platinum*, *gold*, *electrum*, *silver*, *copper* | number | The amount of coins (of each type) being carried. The weight of coins is factored into *encumbrance*. The minimum value is 0. |
| Currency, Banked | *TYPE_bank* | number | The amount of coins (of each type) not carried. The minimum value is 0. |
| Currency, New | *TYPE_found* | number | The balance of coins recently added and/or removed from On Hand. The GM should use this field when adding or deducting coins from the player's account. |
| Claim | *act_sheet_currency* | button | When clicked, adds each *TYPE_found* amount to its On Hand amount and zeros our the New value. Players should use this button to claim changes made to their currentcy pool by the GM. |
|||||
| Living Standard | *living_standard* | list | Contains a list of ACKS living standards. When selected, applies the corresponding monthly cost of living (in gp) to the Monthly Balance, below. The default value is *Adequate*. |
| Hireling Fees | *hireling_fees* | duplicate | See this Hirelings tab. |
| Professional Income | *professional_income* | number | A field for tracking the creature's monthly income (gp) from proficiencies, jobs, etc. |
| Monthly Balance | **monthly_balance* | calculated | A total of the creature's monthly costs shown above, in gp. A positive number is a negative cashflow and should be deducted from the creature's currency every month. |

---

## Hirelings Tab
This tab keeps track of the creature's hirelings and associated fees.

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| Henchmen Limit | *henchmen_limit* | calculated | Auto-calculated as 4 plus the creature's CHA modifier. |
| Hireling Fees | *hireling_fees* | calculated | A subtotal of all monthly fees (gp) owed to the creature's hired henchmen, mercenaries, and specialists. |
|||||
| **Henchmen** | *repeating_henchmen* | repeating | Contains a list of the creature's henchmen. |
| Name | *henchman_name* | text | The name of the henchman. |
| (Loyalty Check) | *roll_henchman_loyalty_check* | button | When clicked, makes a 2d6 loyalty check for the henchman, modified by the creature's CHA modifier and a roll-specific modifier, if any. |
| Loyalty | *henchman_loyalty* | number | The henchman's base loyalty rating to the creature, used to make loyalty checks. Minimum value is -4; maximum is +4, default is 0. |
| (Obedience Check) | *roll_henchman_obedience_check* | button | When clicked, makes a 2d6 obedience check for the henchman, modified by any applicable class bonus and a roll-specific modifier, if any. |
| Morale | *henchman_morale* | number | The henchman's base morale rating, used to make obedience chacks. Minimum value is -4; maximum is +4, default is 0. |
| Class | *henchman_class* | text | The class name of the henchman. |
| Level | *henchman_level* | number | The class level of the henchman. |
| Wage | *henchman_fee* | calculated | The monthly wage that the creature must pay the henchman. Determined based on the henchman level, as per the ACKS henchman wage structure. |
| Notes | *henchman_details* | text | Additional notes about the henchman, typically related to its contract and/or relationship to the creature. |
|||||
| **Mercenaries** | *repeating_mercenaries* | repeating | Contains a list of the creature's hired mercenaries (by group). |
| Name | *merc_name* | text | The name of the mercenary group. |
| Quantity | *merc_number* | number | The number of troops currently within the mercenary group. |
| (Loyalty Check) | *roll_merc_loyalty_check* | button | When clicked, makes a 2d6 loyalty check for the mercenary group, modified by the creature's CHA modifier and a roll-specific modifier, if any. |
| Loyalty | *merc_loyalty* | number | The mercenary group's base loyalty rating to the creature, used to make loyatly checks.  Minimum value is -4; maximum is +4, default is 0. |
| (Obedience Check) | *roll_merc_obedience_check* | button | When clicked, makes a 2d6 obedience check for the mercenary group, modified by any applicable class bonus and a roll-specific modifier, if any. |
| Morale | *merc_morale* | number | The mercenary group's base morale rating, used to make obedience checks. Minimum value is -4; maximum is +4, default is 0. |
| Type | *merc_type* | text | The mercenary group's type. E.G., *Medium Calvary*. |
| Race | *merc_race* | text | The mercenary group's race. E.G., *Goblin*. |
| Wage (per Troop) | *merc_fee* | number | The monthly wage that the creature must pay the mercenary group. Mimimum value is 0; default is 0. |
| Notes | *merc_details* | text | Additional notes about the mercenary group, such as armor and weapons, history, current health, etc. |
|||||
| **Specialists** | *repeating_specialists* | repeating | Contains a list of the creature's hired specialists. |
| Name | *spec_name* | text | The name of the specialist. |
| Type | *spec_type* | text | The specialist's type/occupation. |
| (Loyalty Check) | *roll_spec_loyalty_check* | button | When clicked, makes a 2d6 loyalty check for the specialist, modified by the creature's CHA modifier and a roll-specific modifier, if any. |
| Loyalty | *spec_loyalty* | number | The specialist's base loyalty rating to the creature, used to make loyatly checks.  Minimum value is -4; maximum is +4, default is 0. |
| (Obedience Check) | *roll_spec_obedience_check* | button | When clicked, makes a 2d6 obedience check for the specialist, modified by any applicable class bonus and a roll-specific modifier, if any. |
| Morale | *spec_morale* | number | The specialist's base morale rating, used to make obedience checks. Minimum value is -4; maximum is +4, default is 0. |
| Wage | *spec_fee* | number | The monthly wage that the creature must pay the specialist. Mimimum value is 0; default is 0. |
| Notes | *spec_details* | text | Additional notes about the specialist, typically its description from the ACKS rule book. |

---

## Journal Tab
This tab contains text area fields for tracking additional character information, such as background, appearance, languages, etc. It is also used to log XP and apply prerequisite bonuses to that XP.

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| Character Background | *background* | text | A desciption of the creature's background. Typically used by the player to detail "private" information not published on the character's Roll20 public bio. |
| Appearance | *appearance* | text | A description of the creature's appearance. Might contain hidden details not specified on the creature's public Roll20 bio. |
| Languages | *languages* | text | A list of languages spoken by the creature. |
| Injuries | *injuries* | text | A list of injuries suffered by the creature, typically including any modifiers applied to its performance. |
| Property & Debts | *property* | text | A list of property (not shown on the Equipment tab) that the creature owns, as well as any outstanding debts to others. |
|||||
| **Adventuring Notes** | *repeating_journal* | repeating | A list of notes, managed by the player. |
| Title | *journal_title* | text | A short text description of the note. |
| Details | *journal_details* | text | The details of the note. |
|||||
| **Experience Log** | *repeating_experience* | repeating | A list of all the character's Experience Points, by expedition. |
| Date | *xp_date* | text | The date the XP entry was added to the Experience Log. For reference only. |
| XP | *xp_amount* | number | The amount of XP awarded (without bonuses). |
| Bonus % | *xp_bonus* | number | The percentage of bonus XP awarded, typically from the creature's attribute prime requisite bonus. |
| Note | *xp_description* | text | A short description of the entry. Typically, the name of the module and a session reference. E.G., "Buried Temple, 4th Expedition". |

---

## Settings
This tab contains base values used in some sheet worker calculations; mainly when using the hseet for creatures with non-standard movement and encumbrance capabilites (E.G., mounts, dragons, etc.) **These values should only EVER be changed by the GM.**

| Field | Variable(s) | Type | Description |
| --- | --- | --- | --- |
| Enc Thresholds | *move_thresh1*, *move_thresh2*, *move_thresh3* | number | These fields set the encumbrance thresholds that define decreases in the creature's maximum movement speed. The default values are those for humanoid creatures described in the ACKS rulebook; 5, 7, and 10 stone. These align with movement rate reductions of 1/4, 1/2, and 3/4, respectively. The highest *move_threshX* value to exceed the current capacity is the threshold that will be used. For example, a mule would use 20, 20, and 40; a 20 encumbrance would only trigger 1/2 movement reduction, never 1/4. |
| Max Encumbrance | *base_enc* | number | The creature's base maximum encumbrance threshold, which is the point at which the creature can no longer move. The default is the humanoid maximum base limit of 20 stone. |
| Max Movement | *base_movement* | number | Change to set the maximum movement value per turn for the creature. The default is the humanoid base movement of 120' per turn. |
| Experience % Modifier | *xp_modifier* | number | Used to change the creature's displayed XP bonus from its prime requisite attribute score(s). Typically used if the creature has suffered an organic impediment or injury that nagtively impacts its intellectual / reasoning capability. |

---

## Roll Templates

### 'Custom' Template
The ACKS sheet leverages a custom roll template that supports the following properties:

- *title*: the title to be displayed
- *subtitle*: an optional subtitle to be displayed
- *color*: the default header color is gray, but additional colors may be specified: blakc, brown, blue, gold, green, orange, and red
- *effects*: an optional effects field, used to provide flavor
- *desc*: an optional description field, used to provide details
- *roll* plus *target*: if roll is **less** than target, will indicate *success*; otheriwse, *failure*
- Support for Combat > *roll_surprise* outcomes
- Any additional parameters via the *allprops* roll template feature

### 'Custom' Template Banners
The roll template supports the use of an image banner in its header. However, to respect artist intellectual property, those present in the sheet HTML should be replaced with ones of your own. The list of template headers that can be customized can be found at the end of the sheet HTML just before the sheet worker section. Hopefully, the *name* properties (SOMECONTEXT, below) are intuitive.

![Sample Roll Template Banner](images/roll_template.png)

Use the following syntax to embed your own banner header graphics:

> \<input type='hidden' name='attr_banner_SOMECONTEXT' value='\[x](BANNERURL#.png)'/>

If the banners are located in your Roll20 library, you can obtain its *Banner_URL* by dragging it to any Roll20 page, selecting it, and pressing SHFT+Z to display it in the Roll20 image modal view. Then, right click the image and choose *Copy image address*. Replace the *BANNERURL* portion, abover, with the copied URL (keep the '\[x]', the parentheses, and the '#.png').

I recommend that banners be 300px-by-80px and PNG format.

---

## Supporting Macros
The following macros can be found in the /macros sub-directory of the git. They are *highly* recommended:

* **actions**: uses the Roll20 [ChatMenu](https://app.roll20.net/forum/post/7474530/script-call-for-testers-universal-chat-menus/?pagenum=1) API script to whisper a list of nmany sheet buttons into the Roll20 chat. Buttons are organzied by type. Note that melee and missile attacks will only appear in the output if they've been marked as "equipped" This script saves the players a **lot** of time and is highly recommended. Specify 'Show as Token Action' to apply the macro to every sheet token. Select the token, and then click the macro to use it. (The corallary here is that a token **must** be selected for many of the sheet buttons to work, with our without the macro...)

* **conditions**: uses the Roll20 [TokenMod](https://wiki.roll20.net/Script:Token_Mod) API add-on to apply/remove ACKS condition token icons with a click of a button. Note that my version relies on additional custom icons that I created and imported into the game (see the \images folder for a PNG of these), but the script can be modified to use only the stock icons, as desired.

* **GMRoll**: A simple macro that allows the GM to send a custom, formatted whisper roll to the Roll20 Chat, including a Reason for the throw, a Roll 20 dice formula to be rolled, and an optional Target value. The GM can whisper themself (by default), or a specific party member. Intended to be used to track secret GM throws in a gracefull chat format.

* **initToken**: uses the Roll20 [TokenMod](https://wiki.roll20.net/Script:Token_Mod) to pre-configure a new token that's already bound to a character sheet. I used to standardize tokens - make sure game-wide token settings are already configured. The current macro sets token size to 85% of 1 unit (usually 70 px), and binds the three bar fields to *hp* (and *hp_max*), *move_round*, and *armor_class*. This can be customized anyway the user seems fit. See the TokenMod documentation for more settings.

* **initMonster**: intended to be applied *after* initToken. This will use [TokenMod](https://wiki.roll20.net/Script:Token_Mod) to decouple the token's bar1 binding from the *hp* field, randomize the token's *hp*=*hp_max* to the bound character sheet's *hit_dice* value, add 60' nightvision to the token, and append a random 3-digit number to the token's name. As with initToken, this can be further customized, as desired.

* **repertoire**: also uses [ChatMenu](https://app.roll20.net/forum/post/7474530/script-call-for-testers-universal-chat-menus/?pagenum=1) API script to output the character's repertoire (*spell_prepared* checkbox = on) to the Roll20 chat as clickable links. This macro should be added to specific character sheets via the Roll20 Attributes & Abilities tab.

* **spellbook**: same as for repertoire, above, but outputs a list of all spells in the character's spell list, regardless of *spell_prepared* status.

---

## Supporting API Scripts
The following scripts can be found in the /scripts directory of the git. They are also *highly* recommended:

* **AddActions** : Invoked by selecting one or more tokens on the map and then typing '!AddActions' in the Roll20 chat window. This will add 'generic' melee and missile attacks to each selected tokens' character sheet > Combat > Melee/Missile Attacks repeating lists. For example, Punch, Kick, Improvised, etc.

* **AddSkills**: Invoked by selecting one or more tokens on the map and then typing '!AddSkills' in the Roll20 chat window. This will add common ACKS abilities to each selected tokens' character sheet > Skills > Abilities repeating list. For example, Find Secret Doors, Hear Noise, etc.

### Author's Note
These scripts were derived from the Roll20 forums. I did not author this script and do not recall who on the forums did. If anyone knows, please contact me so I can provide the proper credit.

---

## Built With

* [Roll20](https://roll20.net/)
* [VS Code](https://code.visualstudio.com/)
* [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

---

## Authors

* **omonubi** - *Initial work and all updates to-date.* - [omonubi.com](https://www.omonubi.com)

---

## License

This project is currently not licensed in the git.

The ACKS intellectual property contained within the character sheet is covered under the ACKS SRD/OGL and used with full permission of Alexander Macris, Autuarch.

---

## Acknowledgments

* Alexander Macris, author of ACKS and owner of Autarch
* GiGs, TheAaron, Kraynic, and everyone else in the [Roll20 Forums](https://app.roll20.net/forum/) that have helped me progress to where I am in sheet development over the years.
* My Wednesday night ACKS gaming group, who have put up with me for far too long. ;)
