---
share: true
---
```dataviewjs
const result = await dv.query(`LIST WHERE contains(file.folder, this.file.folder) AND startswith(file.name, "0")`);

const pages = [];
if (result.successful) {
	for (const _page of result.value.values) {
		const page = dv.page(_page);
		const uwp = page.uwp.split(" ").filter((s) => s).map((s) => s.trim());
		let _routes;
		if (page.routes) {
			_routes = page.routes.split(",").filter((r) => r).map((r) => [uwp[1], r.trim()].sort());
		}
		const notes = uwp.slice(3);
		const tradeCodesEndIndex = notes.findIndex((n) => n.length === 1 || n.length === 3);
		const tradeCodes = notes.splice(0, tradeCodesEndIndex);
		const travelCode = notes[0].length === 1 ? notes.shift() : " ";
		pages.push([`[[${page.file.path}|${uwp[0]}]]`, uwp[1], uwp[2].substr(9), uwp[2].substr(0, 9), tradeCodes.join(" "), travelCode, notes.join(" ")]);
	}
}

dv.table(["Name", "Location", "Bases", "Profile", "Trade codes", "Travel Code", "Notes"], pages);
```
