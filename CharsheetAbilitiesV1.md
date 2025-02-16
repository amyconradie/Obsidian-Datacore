```datacorejsx

let styles = {

	checkDice: {
		fontSize:"20px", 
		height:"40px", 
		alignItems:'center', 
		display:'flex', 
		justifyContent:'center', 
		position:'relative', 
		left:'0px', 
		top:'0px'
	},

	skillDice: {
		fontSize:"14px", 
		height:"40px", 
		alignItems:'center', 
		display:'flex', 
		justifyContent:'center', 
		position:'relative', 
		left:'0px', 
		top:'8px'
	},
	
	label: {
		opacity:'0.5',
		fontSize:'12px',
		textAlign:'center',
		margin:'0', 
		position:"relative", 
		left:'0px', 
		top:'-10px'
	},
	
	abilityScore: {
		display:'flex', 
		fontSize:'80px', 
		position:"relative", 
		top:"7px", 
		left:'-10px'
	},
	
	abilityDice: {
		display:'grid', 
		position:'relative', 
		top:'10px', 
		left:'5px',  
		alignContent: 'center', 
		justifyContent:'center'
	},

	skillText: {
		opacity:'0.5',
		fontSize:'12px', 
		textAlign:'center', 
		marginTop:'20px', 
		marginBottom:'-10px' 
	},

	skillDisplay: {
		display:'flex', 
		fontSize:'14px', 
		justifyContent:'space-between', 
		marginBottom:'-20px'
	},

	skillDivider: {
		margin: '0px'
	},

	passiveDivider: {
		margin: '0px',
		marginTop: '20px'
	},
	
	passiveText: {
		opacity:'0.5',
		fontSize:'12px', 
		textAlign:'center', 
		margin:'0px', 
		marginTop:'20px', 
		marginBottom:'-10' 
	},
	
	passiveDisplay: { 
		display:'flex', 
		height:"45px", 
		fontSize:'14px', 
		alignItems:'center', 
		justifyContent:'space-between', 
		marginBottom:'-12px'
	},
	
	abilityCard: {
		width:'220px', 
		height:'fit-content',
		backgroundColor:'var(--background-secondary)', 
		borderRadius:'10px', 
		padding:'24px' 
	},
	
	abilityTitle: { 
		textAlign:'center', 
		fontSize:'20px', 
		marginBottom:'-10px'
	},

	leftAbilityCol: {
		float:'left', 
		padding:'8px'
	},

	rightAbilityCol: {
		float:'right', 
		padding:'8px'
	},
	
	leftAbilityColPadding: {
		height:"15px"
	},
	
	rightAbilityColPadding: {
		height:"13px"
	}
	  
}

// minor components

const Dice = ({ diceLabel, diceStr, diceStyle }) => {
	return (
		<div>
			<div style={diceStyle}>
				<p><dc.Markdown content={diceStr}/></p>
			</div>
			<p style={styles.label}>{diceLabel}</p>
		</div>
	);
};

const AbilityScore = ({ abilityScore, checkStr, saveStr }) => {
	return (
		<div style={{ display:'flex', justifyContent:'center' }}>
			<div>
				<div style={styles.abilityScore}>{abilityScore}</div>
				<p style={styles.label}>score</p>
			</div>
			<div style={styles.abilityDice}>
				<Dice diceLabel={'check'} diceStr={checkStr} diceStyle={styles.checkDice}/>
				<Dice diceLabel={'save'} diceStr={saveStr} diceStyle={styles.checkDice}/>
			</div>
		</div>
	);
};

const SkillCard = ({ abilitySkills, skillDivider = "", skillText = ""}) => {
	
	if (abilitySkills.length > 0) {
		skillDivider = <hr class="solid" display="none" style={styles.skillDivider}/>;
		skillText = <p style={styles.skillText}>additional skills</p>;
	}
	
	return (
		<div>
			{skillDivider}
			{abilitySkills.map(skill => {
				return (
					<div style={styles.skillDisplay}>
						<p>{skill.name}</p>
						<Dice diceStr={skill.str} diceStyle={styles.skillDice}/>
					</div>
				);
			})}
			{skillText}
		</div>
	);
};

const PassiveCard = ({ passiveSkills, passiveDivider = "", passiveText = ""}) => {

	if (passiveSkills.length > 0) {
		passiveDivider = <hr class="solid" display="none" style={styles.passiveDivider}/>;
		passiveText = <p style={styles.passiveText}>passive senses</p>;
	}

	return (
		<div>
			{passiveDivider}
			{passiveSkills.map(passive => { 
				return (
					<div>
						<div style={styles.passiveDisplay}>
							<p>{passive.name}</p>
							<p>{passive.score}</p>
						</div>
					</div>
				)}
			)}
			{passiveText}
		</div>
	);
};

const AbilityCard = ({ability, skills=[], passives = []}) => {	
	return (
		<div style={styles.abilityCard}>
			{<h3 style={styles.abilityTitle}>{ability.name}</h3>}
			{<AbilityScore abilityScore={ability.score} checkStr={ability.checkStr} saveStr={ability.saveStr}/>}
			{<SkillCard abilitySkills={skills}/>}
			{<PassiveCard passiveSkills={passives}/>}
		</div>
	);
};


const AbilitiesCard = ({char}) => {

  return (
	  <div style={{ display: 'flex'}}>
			<div style={styles.leftAbilityCol}>
		        {<AbilityCard ability={char.strength} skills={char.strength.skills}/>}
		        <div style={styles.leftAbilityColPadding}></div>
		        {<AbilityCard ability={char.dexterity} skills={char.dexterity.skills}/>}
		        <div style={styles.leftAbilityColPadding}></div>
		        {<AbilityCard  ability={char.intelligence} skills={char.intelligence.skills} passives={char.intelligence.passives}/>}
			</div>
			<div style={ styles.rightAbilityCol }>
		        {<AbilityCard ability={char.constitution}/>}
		        <div style={styles.rightAbilityColPadding}></div>
		        {<AbilityCard ability={char.wisdom} skills={char.wisdom.skills} passives={char.wisdom.passives}/>}
		        <div style={styles.rightAbilityColPadding}></div>
		        {<AbilityCard ability={char.charisma} skills={char.charisma.skills}/>}
			</div>
	</div>
  );
};

// App

function CharUI() {

	const temp = {
	
		strength: { 
			name: 'STRENGTH', 
			score: 13, 
			checkStr:"\`dice: 1d20+1|nodice|text(+1)\`", 
			saveStr:"\`dice: 1d20+1|nodice|text(+1)\`",
			skills:[{ name: 'Athletics', str:"\`dice: 1d20+4|nodice|text(+4)\`" }]
		},
		
		dexterity: { 
			name: 'DEXTERITY', 
			score: 10, 
			checkStr:"\`dice: 1d20|nodice|text(+0)\`", 
			saveStr:"\`dice: 1d20|nodice|text(+0)\`" ,
			skills:[
			  { name: 'Acrobatics', str:"\`dice: 1d20+0|nodice|text(+0)\`"  },
			  { name: 'Sleight of Hand', str:"\`dice: 1d20+0|nodice|text(+0)\`"  },
			  { name: 'Stealth', str:"\`dice: 1d20+0|nodice|text(+0)\`"  }
			]
		},
		
		constitution: { 
			name: 'CONSTITUTION', 
			score: 15, 
			checkStr:"\`dice: 1d20+2|nodice|text(+2)\`", 
			saveStr:"\`dice: 1d20+2|nodice|text(+2)\`" 
		},
		
		intelligence: { 
			name: 'INTELLIGENCE', 
			score: 12, 
			checkStr:"\`dice: 1d20+1|nodice|text(+1)\`", 
			saveStr:"\`dice: 1d20+1|nodice|text(+1)\`" ,
			skills:[
			  { name: 'Arcana', str:"\`dice: 1d20+1|nodice|text(+1)\`"  },
			  { name: 'History', str:"\`dice: 1d20+1|nodice|text(+1)\`"  },
			  { name: 'Investigation', str:"\`dice: 1d20+1|nodice|text(+1)\`"  },
			  { name: 'Nature', str:"\`dice: 1d20+1|nodice|text(+1)\`"  },
			  { name: 'Religion', str:"\`dice: 1d20+1|nodice|text(+1)\`"  }
			],
			passives: [{ name: 'Passive Investigation', score: 11 }]
		},
		wisdom: { 
			name: 'WISDOM', 
			score: 18, 
			checkStr:"\`dice: 1d20+4|nodice|text(+4)\`", 
			saveStr:"\`dice: 1d20+7|nodice|text(+7)\`" ,
			skills:[
			  { name: 'Animal Handling', str:"\`dice: 1d20+7|nodice|text(+7)\`"  },
			  { name: 'Insight', str:"\`dice: 1d20+4|nodice|text(+4)\`"  },
			  { name: 'Medicine', str:"\`dice: 1d20+7|nodice|text(+7)\`"  },
			  { name: 'Perception', str:"\`dice: 1d20+2|nodice|text(+2)\`"  },
			  { name: 'Survival', str:"\`dice: 1d20+7|nodice|text(+7)\`"  }
			],
			passives: [
				{ name: 'Passive Insight', score: 17 }, 
				{ name: 'Passive Perception', score: 17 }
			]
		},
		charisma: { 
			name: 'CHARISMA', 
			score: 8, 
			checkStr:"\`dice: 1d20-1|nodice|text(-1)\`", 
			saveStr:"\`dice: 1d20+2|nodice|text(+2)\`" ,
			skills:[
			  { name: 'Deception', str:"\`dice: 1d20-1|nodice|text(-1)\`"  },
			  { name: 'Intimidation', str:"\`dice: 1d20-1|nodice|text(-1)\`"  },
			  { name: 'Performance', str:"\`dice: 1d20-1|nodice|text(-1)\`"  },
			  { name: 'Persuasion', str:"\`dice: 1d20-1|nodice|text(-1)\`"  }
			]
		}
	};
	
	// Main Layout
	return (
		<div>
			{<AbilitiesCard char={temp}/>}
		</div>
	);
}

return CharUI;
```
