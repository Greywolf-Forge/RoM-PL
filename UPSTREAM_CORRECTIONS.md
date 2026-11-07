# RoM-PL: upstream English corrections

## For Mod Authors

Greywolf Forge supplies narrowly scoped, source-verified corrections to the English texts shipped with this mod. Each change below can be applied independently when the original fragment and source revision match. No gameplay code or data is changed.

The canonical catalogue is [`patches/english-corrections-v0.3.json`](patches/english-corrections-v0.3.json). It binds each change to a source file, Tag, zero-based occurrence and SHA-256. The complete corrected English entries are in [`upstream/ZZZ_GreywolfForge_English.xml`](upstream/ZZZ_GreywolfForge_English.xml). These are exchange records for merging by Tag and occurrence, not a second drop-in runtime overlay.

Historical guides keep their original version context, author voice, examples and structure. We do not replace an older ruleset with current mod mechanics merely because the values differ. Unverified historical claims and optional stylistic changes are listed separately below.

For questions or counterexamples, please open an issue in [the RoM-PL repository](https://github.com/Greywolf-Forge/RoM-PL/issues), including the correction ID, mod version and relevant source.

## Scope

Target: **rev1093**, Civilization IV: Beyond the Sword 3.19.

Confirmed corrections: **6**, affecting **6 source records**. This is a targeted audit, not a claim that all English text has been proofread.

## Confirmed corrections

### naming-counter-tt2

- **Tag:** `TXT_KEY_BUG_OPT_UNITNAMING__DEFAULT_HOVER`
- **Source:** `INTERFACE.xml`, occurrence `0` (zero-based)
- **Category:** `MECHANICS_ERROR`
- **Version:** rev1093

**Original English fragment:**

```text
tt1[f][x][COLOR_REVERT] - Count from x
```

**Corrected English fragment:**

```text
tt2[f][x][COLOR_REVERT] - Count from x
```

The secondary counter is tt2, while tt1 has a range argument [x:y]. The example must use the token accepted by getTotal2 and swapCountCode. This changes the documented code, not the parser.

**Verification:**

- Assets/Python/Contrib/UnitNameEventManager.py; getTotal2; swapCountCode
  SHA-256: `4b9921d6195b379168d0ac83fd851fb7ca3a426ba3c7db2a4060d91102474b13`.
  getTotal2 finds "^tt2["; swapCountCode(zsName, "^tt2", ziTT2).

**Compatibility:** Applies to this installed mod version; do not replace base BtS civic names globally. Text only; no game data changes.

### trade-naming-option

- **Tag:** `TXT_KEY_BUG_OPT_UNITNAMING__COMBAT_TRADE_HOVER`
- **Source:** `INTERFACE.xml`, occurrence `0` (zero-based)
- **Category:** `FACTUAL_ERROR`
- **Version:** rev1093

**Original English fragment:**

```text
'EXECUTIVES'
```

**Corrected English fragment:**

```text
'TRADE'
```

The option and UI binding are Combat_TRADE. Combat_EXECUTIVES is a separate setting; copying its name into this tooltip describes the wrong control.

**Verification:**

- Assets/Config/Unit Naming.xml; option[@id="Combat_TRADE"] and option[@id="Combat_EXECUTIVES"]
  SHA-256: `837731e20c9098d220d7d5b39b73b9307399ee63c4f6fd0d69adaf56952c2939`.
- Assets/Python/BUG/Tabs/BugUnitNameOptionsTab.py; create
  SHA-256: `e523082be6a9c8403c407b62a1cd36afcf116234a8da280af2399cb0388d1b22`.
  self.addTextEdit(screen, columnL, columnR, "UnitNaming__Combat_TRADE")

**Compatibility:** Applies to this installed mod version; do not replace base BtS civic names globally. Text only; no game data changes.

### hostile-takeover-name

- **Tag:** `TXT_KEY_EVENTTRIGGER_HOSTILE_TAKEOVER_DONE`
- **Source:** `EVENTS.xml`, occurrence `0` (zero-based)
- **Category:** `FACTUAL_ERROR`
- **Version:** rev1093

**Original English fragment:**

```text
quest of Corporate Expansion
```

**Corrected English fragment:**

```text
quest of Hostile Takeover
```

EVENTTRIGGER_HOSTILE_TAKEOVER_DONE uses this world-news text after the Hostile Takeover quest. Corporate Expansion is a different quest. Parameters and the rest of the completion message are preserved.

**Verification:**

- Assets/XML/Events/CIV4EventTriggerInfos.xml; EVENTTRIGGER_HOSTILE_TAKEOVER_DONE/WorldNewsTexts
  SHA-256: `be139363f1e8db3c04582978e8879b4d34dc7a5b99059417f0527bba5ffdc1fb`.
  TXT_KEY_EVENTTRIGGER_HOSTILE_TAKEOVER_DONE
- Assets/Python/EntryPoints/CvRandomEventInterface.py; canTriggerHostileTakeoverDone
  SHA-256: `36983bfe00ac7e1f37ef48bc5d7a661c7e3f34bfd04685e9a1bf515eae8cf573`.
  def canTriggerHostileTakeoverDone(argsList): 	kTriggeredData = argsList[0] 	player = gc.getPlayer(kTriggeredData.ePlayer) 	trigger = gc.getEventTriggerInfo(kTriggeredData.eTrigger) 	kOrigTriggeredData = player.getEventOccured(trigger.getPrereqEvent(0))  	listResources = getHostileTakeoverListResources(gc.getCorporationInfo(kOrigTriggeredData.eCorporation), player)  	if len(listResources) > 0: 		return False  	kActualTriggeredDataObject = player.getEventTriggered(kTriggeredData.iId) 	kActualTriggeredDataObject.eCorporation = kOrigTriggeredData.eCorporation 	kActualTriggeredDataObject.eBuilding = kOrigTriggeredData.eBuilding 	kActualTriggeredDataObject.iCityId = kOrigTriggeredData.iCityId 	kActualTriggeredDataObject.iPlotX = kOrigTriggeredData.iPlotX 	kActualTriggeredDataObject.iPlotY = kOrigTriggeredData.iPlotY  	return True

**Compatibility:** Applies to this installed mod version; do not replace base BtS civic names globally. Text only; no game data changes.

### guns-butter-civic

- **Tag:** `TXT_KEY_EVENT_GUNS_BUTTER_HELP_1`
- **Source:** `EVENTS.xml`, occurrence `0` (zero-based)
- **Category:** `MECHANICS_ERROR`
- **Version:** rev1093

**Original English fragment:**

```text
[COLOR_HIGHLIGHT_TEXT]Vassalage[COLOR_REVERT]
```

**Corrected English fragment:**

```text
[COLOR_HIGHLIGHT_TEXT]Nobility[COLOR_REVERT]
```

canApplyGunsButterDone2 tests CIVIC_ARISTOCRACY. Its Description is TXT_KEY_CIVIC_ARISTOCRACY, whose English label is Nobility in this mod. Vassalage is the old BtS label and names the wrong mod civic.

**Verification:**

- Assets/Python/EntryPoints/CvRandomEventInterface.py; canApplyGunsButterDone2
  SHA-256: `36983bfe00ac7e1f37ef48bc5d7a661c7e3f34bfd04685e9a1bf515eae8cf573`.
  def canApplyGunsButterDone2(argsList): 	iEvent = argsList[0] 	kTriggeredData = argsList[1] 	player = gc.getPlayer(kTriggeredData.ePlayer)  	iCivic = gc.getInfoTypeForString("CIVIC_ARISTOCRACY")  	if not player.isCivic(iCivic): 		return False  	return True
- Assets/XML/GameInfo/CIV4CivicInfos.xml; CIVIC_ARISTOCRACY/Description
  SHA-256: `250c34565270ec17f0c97cbfdeb1b51e0066ed5c6199c2e62c611f4b361eda67`.
  TXT_KEY_CIVIC_ARISTOCRACY
- Assets/XML/Text/CIVICS.xml; TXT_KEY_CIVIC_ARISTOCRACY/English
  SHA-256: `1fb30b8b1d67b4cc4680840958e7a7740b1655bb8acc04b92b018ddfe6ef0b99`.
  Nobility

**Compatibility:** Applies to this installed mod version; do not replace base BtS civic names globally. Text only; no game data changes.

### industrial-action-civic

- **Tag:** `TXT_KEY_EVENT_INDUSTRIAL_ACTION_2_HELP`
- **Source:** `EVENTS.xml`, occurrence `0` (zero-based)
- **Category:** `MECHANICS_ERROR`
- **Version:** rev1093

**Original English fragment:**

```text
[COLOR_NEGATIVE_TEXT]State Property[COLOR_REVERT]
```

**Corrected English fragment:**

```text
[COLOR_NEGATIVE_TEXT]Planned[COLOR_REVERT]
```

canDoIndustrialAction2 excludes CIVIC_PLANNED. Its Description resolves to the English label Planned. State Property is an obsolete base-game reference. The event condition is unchanged.

**Verification:**

- Assets/Python/EntryPoints/CvRandomEventInterface.py; canDoIndustrialAction2
  SHA-256: `36983bfe00ac7e1f37ef48bc5d7a661c7e3f34bfd04685e9a1bf515eae8cf573`.
  def canDoIndustrialAction2(argsList):  	iEvent = argsList[0] 	kTriggeredData = argsList[1] 	pPlayer = gc.getPlayer(kTriggeredData.ePlayer)  	if pPlayer.isCivic(gc.getInfoTypeForString("CIVIC_PLANNED")): 		return False  	return True
- Assets/XML/GameInfo/CIV4CivicInfos.xml; CIVIC_PLANNED/Description
  SHA-256: `250c34565270ec17f0c97cbfdeb1b51e0066ed5c6199c2e62c611f4b361eda67`.
  TXT_KEY_CIVIC_PLANNED
- Assets/XML/Text/CIVICS.xml; TXT_KEY_CIVIC_PLANNED/English
  SHA-256: `1fb30b8b1d67b4cc4680840958e7a7740b1655bb8acc04b92b018ddfe6ef0b99`.
  Planned

**Compatibility:** Applies to this installed mod version; do not replace base BtS civic names globally. Text only; no game data changes.

### synthetic-fuels-resource

- **Tag:** `TXT_KEY_LOCAL_SYNTHETIC_FUELS_4`
- **Source:** `MISC.xml`, occurrence `0` (zero-based)
- **Category:** `MECHANICS_ERROR`
- **Version:** rev1093

**Original English fragment:**

```text
+1 oil resource
```

**Corrected English fragment:**

```text
+1 Oil Products resource
```

EVENT_SYNTHETIC_FUELS_4 calls doSyntheticFuels4 and displays this LocalInfoText. The callback grants one BONUS_DIESEL, whose English Description is Oil Products, not Oil.

**Verification:**

- Assets/Python/EntryPoints/CvRandomEventInterface.py; doSyntheticFuels4
  SHA-256: `36983bfe00ac7e1f37ef48bc5d7a661c7e3f34bfd04685e9a1bf515eae8cf573`.
  def doSyntheticFuels4(argsList): 	iEvent = argsList[0] 	kTriggeredData = argsList[1]  	pPlayer = gc.getPlayer(kTriggeredData.ePlayer) 	pCity = pPlayer.getCity(kTriggeredData.iCityId)  	iBonus = gc.getInfoTypeForString("BONUS_DIESEL")  	pCity.changeFreeBonus(iBonus, 1)  	return 1   ####### ALTERNATIVE_ENERGY ######
- Assets/XML/Events/CIV4EventInfos.xml; EVENT_SYNTHETIC_FUELS_4
  SHA-256: `9498071ca99670fc8ee95a6f1347065935a71877ba321eaa93453f7186e1fc83`.
  PythonCallback=doSyntheticFuels4; LocalInfoText=TXT_KEY_LOCAL_SYNTHETIC_FUELS_4
- Assets/XML/Terrain/CIV4BonusInfos.xml; BONUS_DIESEL/Description
  SHA-256: `a387087a44ed4396725e68d368d77f9ed0c397ebcb570145586732bc0664f2b9`.
  TXT_KEY_BONUS_DIESEL
- Assets/XML/Text/RESSOURCES.xml; TXT_KEY_BONUS_DIESEL/English
  SHA-256: `4075b972d055cbc57212d8b092e904d5f29d1f21520a37868db8048f55bd4f99`.
  Oil Products

**Compatibility:** Applies to this installed mod version; do not replace base BtS civic names globally. Text only; no game data changes.

## Findings left unchanged

- `TXT_KEY_EVENT_ALTERNATIVE_ENERGY_4_HELP` — FALSE_POSITIVE_AS_DESIGNED: CIVIC_ENVIRONMENTALISM is the internal Type, but its Description is TXT_KEY_CIVIC_GREEN and the English label is Green. The English civic name is correct. No English correction proposed.
- `TXT_KEY_EVENT_EARTHDAY_HELP_2` — FALSE_POSITIVE_AS_DESIGNED: CIVIC_ENVIRONMENTALISM is the internal Type, but its Description is TXT_KEY_CIVIC_GREEN and the English label is Green. The English civic name is correct. No English correction proposed.
- `TXT_KEY_EVENT_ELITE_SWORDS_HELP_1` — FALSE_POSITIVE_AS_DESIGNED: CIVIC_HEREDITARY_RULE resolves through Description=TXT_KEY_CIVIC_MONARCHY to English Monarchy in this mod. The internal identifier is not the displayed civic name. No correction proposed for that label.
- `TXT_KEY_EVENT_ALTERNATIVE_ENERGY_HELP_1` — REQUIRES_FURTHER_AUDIT: Green is correct; era-based expiry needs comparison to exact obsolete technologies. Not yet a confirmed correction in this proposal.
- `TXT_KEY_EVENT_SPORTS_LEAGUE_HELP_1` — REQUIRES_FURTHER_AUDIT: Do not generalize from era labels; verify exact obsolete technology mapping before changing expiry wording.
- `TXT_KEY_FEAT_NATIONAL_WONDER` — REQUIRES_FURTHER_AUDIT: Previous evidence superseded the fixed 6/8 approach. Requires active culture-level getter verification, especially CoM.
- `TXT_KEY_HINT_28` — REQUIRES_FURTHER_AUDIT: Previous evidence superseded the fixed 6/8 approach. Requires active culture-level getter verification, especially CoM.

## Editorial suggestions

No optional stylistic rewrites are included. 0 suggestions are recorded outside this release.

## Provenance and rights

Greywolf Forge corrections are covered by this repository’s license. Original mod text, quoted article text and earlier translations remain the work of their respective authors; this project does not relicense those underlying works.
