# Future Integration: spreadsheet-moment-proto

## Current State
SpreadsheetMoment's proof-of-concept — reimagining the spreadsheet as a distributed system of intelligent agents. Cells are autonomous agents that connect to hardware sensors, query APIs, run ML models, and coordinate with each other while maintaining the familiar grid interface. Based on peer-reviewed SuperInstance research.

## Integration Opportunities

### With superinstance-spreadsheet
The proto IS the proof-of-concept that led to superinstance-spreadsheet's browser demo. Now superinstance-spreadsheet has the full ternary engine; the proto provides the UX concepts: drag-and-drop simplicity, visual evolution controls, and the "cell = agent" metaphor that makes the system accessible to non-programmers.

### With room-as-codespace
The proto's concept of cells-as-autonomous-agents maps directly to rooms-as-collections-of-ensigns. Each room is a spreadsheet where cells are specialist agents. The proto's UX insights (how to visualize agent state, how to make autonomous behavior understandable) inform the room's frontend design.

### With ternary-spreadsheet
The proto demonstrated the concept. ternary-spreadsheet provides the production implementation. The bridge: take the proto's visual language (fitness coloring, species indicators, evolution animations) and implement them in the ternary-spreadsheet's Univer-based frontend.

## Dormant Ideas Now Unlockable
The "drag-and-drop agent programming" concept was ahead of the runtime. Now construct-core's skill system and ternary-registry's capability declarations provide the programmatic backend. Users can drag skills onto cells to equip them — the proto's UX vision made real.

## Potential in Mature Systems
The proto's vision becomes the fleet's primary interface. Non-technical users manage rooms by dragging formulas onto cells, connecting cells with edges, and watching evolution happen. The spreadsheet metaphor makes autonomous agent systems accessible to everyone.

## Cross-Pollination Ideas
- **Spreadsheet-moment**: Proto's UX concepts → production Univer frontend
- **Equipment-CellLogic-Distiller**: Distiller's tile decomposition → proto's cell-agent model
- **beta-test repos**: Proto's UX was designed for beta testers; their feedback shapes the final interface

## Dependencies for Next Steps
- Extract UX concepts into reusable components
- Bridge to ternary-spreadsheet's data model
- User testing with beta-test feedback
