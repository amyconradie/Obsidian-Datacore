---
inspiration: false
deathSavesSuccesses: 0
deathSavesFailures: 0
name: Una Dew
species: Hexblood
level: 7
experience: 0
charClasses:
  - cleric
classLevels:
  - "7"
charSubclasses: 
  - Twilight Domain
hitPointsMaximum: 52
hitPointsCurrent: 52
hitPointsTemp: 0
armorClass: 10
initiative: 0
darkvision: 300
blindsight: 0
tremorsense: 0
truesight: 0
defenses:
  - none
strength: 13
dexterity: 10
constitution: 15
intelligence: 12
wisdom: 18
charisma: 8
proficiencies:
  - athletics
  - insight
  - medicine
  - perception
  - religion
  - survival
  - wisdomSave
  - charismaSave
  - common
  - goblin
  - sylvan
  - herbalismKit
  - martialWeapons
  - simpleWeapons
  - heavyArmor
  - lightArmor
  - mediumArmor
  - shields
expertises:
  - none
background: Mother's Minder
alignment: Chaotic Good

gender: Female
skin: Green
eyes: Yellow
hair: Black
height: 5' 11"
faith: Tymora
age: 23
weight: 187

appearance: A tall-ish woman with green skin, black untamed hair, forked ears, and a golden wreath around her head. She has a manic look in her eyes and an over eager smile. Dressed in chain mail, carrying a mace and a shield. In her pocket is a gold coin with a woman's smiling face surrounded by clovers.
bonds: none
flaws: none
ideals: none
traits: none


---

```datacorejsx

function modifierString(mod) { return mod >= 0 ? '+' + mod : '-' + mod; }
function d20Str(mod) { return "\`dice: 1d20" + modifierString(mod) + "|nodice|text("+modifierString(mod)+")\`"; }

class Ability { constructor(score = 10) { this.score = score; } get modifier() { return Math.floor((this.score - 10) / 2); }}
class Language { constructor(isProficient = false) { this.isProficient = isProficient; }}
class WeaponProficiency { constructor(isProficient = false) { this.isProficient = isProficient; } }
class ArmorProficiency { constructor(isProficient = false) { this.isProficient = isProficient; } }
class ToolProficiency { constructor(isProficient = false) { this.isProficient = isProficient; } }
class Sense { constructor(distance = 0) { this.distance = distance; } }
class Condition { constructor(isActive = false) { this.isActive = isActive; } }
class Defense { constructor(defenseLevel = "none") { this.defenseLevel = defenseLevel; }}

class SavingThrow {
    constructor(ability, isProficient = false, isExpert = false) {
        this.ability = ability;
        this.isProficient = isProficient;
        this.isExpert = isExpert;
        this.bonusModifier = 0;
    }

    getModifier(proficiencyBonus) {
		this.modifier = this.ability.modifier + 
               (this.isProficient ? proficiencyBonus : 0) + 
               (this.isExpert ? proficiencyBonus : 0) + 
               this.bonusModifier;
        return this.modifier;
    }
}

class Skill {
    constructor(ability, isProficient = false, isExpert = false) {
        this.ability = ability;
        this.isProficient = isProficient;
        this.isExpert = isExpert;
        this.bonusModifier = 0;
    }

    getModifier(proficiencyBonus) {
		this.modifier = this.ability.modifier + 
               (this.isProficient ? proficiencyBonus : 0) + 
               (this.isExpert ? proficiencyBonus : 0) + 
               this.bonusModifier
        return this.modifier;
    }

    getPassiveCheck() {
		this.passiveCheck = 10 + this.modifier;
        return this.passiveCheck;
    }
}

class Character {
    constructor() {
        this.name = "";
        this.level = 0;
        this.experience = 0;
        this.armorClass = 10;
        this.initiative = 0;
    
        this.background = "";
    
	    // Classes
        this.classes = [];
        this.subclasses = [];
        
	    // Species
        this.species = "";
        this.speed = 30;
        this.size = "medium";
        
        // Character Details
        this.alignment = "neutral";
        this.traits = [];
        this.ideals = [];
        this.bonds = [];
        this.flaws = [];
    
        // Combat stats
        this.hitPoints = {
            maximum: 1,
            current: 1,
            temporary: 0
        };

        // Equipment
        this.inventory = [];
        this.equipment = {
            armor: null,
            mainHand: null,
            offHand: null
        };

        // Core abilities
        this.abilities = {
            strength: new Ability(),
            dexterity: new Ability(),
            constitution: new Ability(),
            intelligence: new Ability(),
            wisdom: new Ability(),
            charisma: new Ability()
        };

        // Saving Throws
        this.savingThrows = {
            strengthSave: new SavingThrow(this.abilities.strength),
            dexteritySave: new SavingThrow(this.abilities.dexterity),
            constitutionSave: new SavingThrow(this.abilities.constitution),
            intelligenceSave: new SavingThrow(this.abilities.intelligence),
            wisdomSave: new SavingThrow(this.abilities.wisdom),
            charismaSave: new SavingThrow(this.abilities.charisma)
		};

        // Skills
        this.skills = {
            acrobatics: new Skill(this.abilities.dexterity),
            animalHandling: new Skill(this.abilities.wisdom),
            arcana: new Skill(this.abilities.intelligence),
            athletics: new Skill(this.abilities.strength),
            deception: new Skill(this.abilities.charisma),
            history: new Skill(this.abilities.intelligence),
            insight: new Skill(this.abilities.wisdom),
            intimidation: new Skill(this.abilities.charisma),
            investigation: new Skill(this.abilities.intelligence),
            medicine: new Skill(this.abilities.wisdom),
            nature: new Skill(this.abilities.intelligence),
            perception: new Skill(this.abilities.wisdom),
            performance: new Skill(this.abilities.charisma),
            persuasion: new Skill(this.abilities.charisma),
            religion: new Skill(this.abilities.intelligence),
            sleightOfHand: new Skill(this.abilities.dexterity),
            stealth: new Skill(this.abilities.dexterity),
            survival: new Skill(this.abilities.wisdom)
        };

        // Weopon Proficiencies
        this.weaponProficiencies = {
            simpleWeapons: new WeaponProficiency(),
            martialWeapons: new WeaponProficiency(),
            fireArms: new WeaponProficiency()
		};

        // Weopon Proficiencies
        this.armorProficiencies = {
            shields: new ArmorProficiency(),
            lightArmor: new ArmorProficiency(),
            mediumArmor: new ArmorProficiency(),
            heavyArmor: new ArmorProficiency()
		};

        // Tool Proficiencies
        this.toolProficiencies = {
            herbalismKit: new ToolProficiency()
		};

        // Languages
        this.languages = {
            common: new Language(),
            goblin: new Language(),
            sylvan: new Language()
		};

        // Senses
        this.senses = {
            darkvision: new Sense(),
            blindsight: new Sense(),
            tremorsense: new Sense(),
            truesight: new Sense()
		};

        // Defenses
        this.defenses = {
            acid: new Defense(),
            cold: new Defense(),
            fire: new Defense(),
            force: new Defense(),
            lightning: new Defense(),
            necrotic: new Defense(),
            poison: new Defense(),
            psychic: new Defense(),
            radiant: new Defense(),
            thunder: new Defense()
		};

        // Conditions
        this.conditions = {
            blinded: new Condition(),
            deafened: new Condition(),
            grappled: new Condition(),
            invisible: new Condition(),
            petrified: new Condition(),
            prone: new Condition(),
            stunned: new Condition(),
            charmed: new Condition(),
            frightened: new Condition(),
            incapacitated: new Condition(),
            paralysed: new Condition(),
            poisoned: new Condition(),
            restrained: new Condition(),
            unconscious: new Condition(),
            exhaustion1: new Condition(),
            exhaustion2: new Condition(),
            exhaustion3: new Condition(),
            exhaustion4: new Condition(),
            exhaustion5: new Condition(),
            exhaustion6: new Condition()
		};

    }

    get proficiencyBonus() { return Math.floor((this.level - 1) / 4) + 2; }
    setAbilityScore(ability, score) { if (this.abilities[ability]) { this.abilities[ability].score = score; }}
	setDiceString(rollName, rollModifier) { this.diceStrings[rollName] = d20Str(rollModifier);	}

    setProficiency(proficiency, isProficient) {
        if (this.skills[proficiency]) {
            this.skills[proficiency].isProficient = isProficient;
        }
        if (this.savingThrows[proficiency]) {
            this.savingThrows[proficiency].isProficient = isProficient;
        }
        if (this.weaponProficiencies[proficiency]) {
            this.weaponProficiencies[proficiency].isProficient = isProficient;
        }
        if (this.armorProficiencies[proficiency]) {
            this.armorProficiencies[proficiency].isProficient = isProficient;
        }
        if (this.toolProficiencies[proficiency]) {
            this.toolProficiencies[proficiency].isProficient = isProficient;
        }
        if (this.languages[proficiency]) {
            this.languages[proficiency].isProficient = isProficient;
        }
    }

	 setDiceStrings() {
		this.diceStrings = {};

		// Abilities
		this.setDiceString("strengthCheck", this.abilities.strength.modifier);
		this.setDiceString("strengthSave", this.savingThrows.strengthSave.getModifier(this.proficiencyBonus));
		this.setDiceString("dexterityCheck", this.abilities.dexterity.modifier);
		this.setDiceString("dexteritySave", this.savingThrows.dexteritySave.getModifier(this.proficiencyBonus));
		this.setDiceString("constitutionCheck", this.abilities.constitution.modifier);
		this.setDiceString("constitutionSave", this.savingThrows.constitutionSave.getModifier(this.proficiencyBonus));
		this.setDiceString("intelligenceCheck", this.abilities.intelligence.modifier);
		this.setDiceString("intelligenceSave", this.savingThrows.intelligenceSave.getModifier(this.proficiencyBonus));
		this.setDiceString("wisdomCheck", this.abilities.wisdom.modifier);
		this.setDiceString("wisdomSave", this.savingThrows.wisdomSave.getModifier(this.proficiencyBonus));
		this.setDiceString("charismaCheck", this.abilities.charisma.modifier);
		this.setDiceString("charismaSave", this.savingThrows.charismaSave.getModifier(this.proficiencyBonus));

		// Skills
		
		
	 }

	loadCharacter(note) {
        this.name = note.value("name");
        this.classes = note.value("classes");
        this.subclasses = note.value("subclasses");
        this.experience = note.value("experience");
        this.level = note.value("level");
        this.species = note.value("species");
        this.background = note.value("background");

		// set ability scores
		this.setAbilityScore("strength", note.value("strength"));
		this.setAbilityScore("dexterity", note.value("dexterity"));
		this.setAbilityScore("constitution", note.value("constitution"));
		this.setAbilityScore("intelligence", note.value("intelligence"));
		this.setAbilityScore("wisdom", note.value("wisdom"));
		this.setAbilityScore("charisma", note.value("charisma"));

		// set proficiencies
		for (let i = 0; i < note.value("proficiencies").length; i++) {
		  this.setProficiency(note.value("proficiencies")[i], true);
		}

		// set character details
        this.alignment = note.value("alignment");
        this.bonds = note.value("bonds");
        this.flaws = note.value("flaws");
        this.ideals = note.value("ideals");
        this.traits = note.value("traits");

		this.setDiceStrings();
	}

	

}

// Example usage
const characterSheet = new Character();

const AbilityTable = ({check}) =>  {

	const current = dc.useCurrentFile();
	characterSheet.loadCharacter(current);
	strCheck = characterSheet.diceStrings["strengthCheck"];
	
	 return (		 
		<table>
	        <thead>
	            <tr>
	                <th>Ability</th>
	                <th>Score</th>
	                <th>Check</th>
	                <th>Save</th>
	            </tr>
	        </thead>
	        <tbody>
	            <tr>
	                <td>Strength</td>
	                <td>{characterSheet.abilities.strength.score}</td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["strengthCheck"]}/></p></td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["strengthSave"]}/></p></td>
					<td><p><dc.Markdown content={strCheck}/></p></td>
					<td><p><dc.Markdown content={check}/></p></td>
	            </tr>
	            <tr>
	                <td>Dexterity</td>
	                <td>{characterSheet.abilities.dexterity.score}</td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["dexterityCheck"]}/></p></td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["dexteritySave"]}/></p></td>
	            </tr>
	            <tr>
	                <td>Constitution</td>
	                <td>{characterSheet.abilities.constitution.score}</td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["constitutionCheck"]}/></p></td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["constitutionSave"]}/></p></td>
	            </tr>
	            <tr>
					<td>Intelligence</td>
	                <td>{characterSheet.abilities.intelligence.score}</td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["intelligenceCheck"]}/></p></td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["intelligenceSave"]}/></p></td>
	            </tr>
	            <tr>
					<td>Wisdom</td>
	                <td>{characterSheet.abilities.wisdom.score}</td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["wisdomCheck"]}/></p></td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["wisdomSave"]}/></p></td>
	            </tr>
	            <tr>
					<td>Charisma</td>
	                <td>{characterSheet.abilities.charisma.score}</td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["charismaCheck"]}/></p></td>
					<td><p><dc.Markdown content={characterSheet.diceStrings["charismaSave"]}/></p></td>
	            </tr>
	        </tbody>
	    </table>
	    
	);
};

function CharSheetUI() {
  // Main Layout
	return (
		<AbilityTable check={"\`dice: 1d20\`"}/>
	);
};

return CharSheetUI()
```
