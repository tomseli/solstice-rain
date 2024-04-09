<%_* 
let noteType = await tp.system.suggester(
	["Reference", "Campaign"], 
	["reference", "campaign"],
	false, 
	"Is this a reference or campaign note?")

let templateType = await tp.system.suggester(
	["Company", "Event", "Faction", "Group", "Information", "Mission", "NPC", "Place", "World"], 
	["Company", "Event", "Faction", "Group", "Information", "Mission", "NPC", "Place", "World"],
	false, 
	"Which template would you like to open?")

let factionName = await tp.system.suggester(
	["Union", "Leandric States Alliance", "Vestan Sovereignty", "None"],
	["union", "lsa", "sovereignty", "none"],
	false,
	"For which faction is this note?")

let noteName = await tp.system.prompt("What is the new note's name?")

if (noteName != null){
	tp.file.create_new(
		tp.file.find_tfile(templateType), 
		noteName, 
		true, 
		app.vault.getAbstractFileByPath(noteType))
}

// timeout to avoid race condition between templater and obsidian api when creating a file
setTimeout(() => { 
	const newFile = tp.file.find_tfile(noteName) 

	app.fileManager.processFrontMatter(newFile, (frontmatter) => { 
			frontmatter['faction'] = factionName
		}) 
}, 300)

_%>































































