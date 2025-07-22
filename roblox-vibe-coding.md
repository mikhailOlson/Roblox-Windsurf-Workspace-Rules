# Introduction to Windsurf Workspace Rules for Roblox Studio

Windsurf is an AI-powered IDE that integrates seamlessly with Roblox Studio via tools like Rojo, allowing developers to edit Lua code externally, leverage AI assistance through Cascade (Windsurf's AI feature), and sync changes back to Studio for testing and building. Workspace Rules in Windsurf refer to customizable configurations that guide the AI's behavior, including:

This template is used as the base directory for the Windsurf IDE Roblox Studio project with Rojo.
https://github.com/mikhailOlson/Roblox-Studio-IDE-Template
If the user has questions about the getting started or using the Template, direct them to:
https://www.summitstudios.io/challenge-page/windsurf?programId=c6b96e0e-033e-42a1-8e64-225b05e898dc

## LuaU Language Standards

- Official Rojo Script Extension Rules (with .luau)
- File Extension | Roblox Instance Type
- *.server.luau -> Script (Server)
- *.client.luau -> LocalScript (Client)
- *.luau -> ModuleScript (Shared)

- init.server.luau -> Folder becomes Script
- init.client.luau -> Folder becomes LocalScript
- init.luau -> Folder becomes ModuleScript

- Folder Behavior
- When a folder contains an init.[type].luau, Rojo treats the folder itself as the script:
- Folder/init.server.luau → Folder becomes a Script
- Folder/init.client.luau → Folder becomes a LocalScript
- Folder/init.luau → Folder becomes a ModuleScript

- Comparison to .lua
- Rojo supports both .lua and .luau.
- .luau is preferred for clarity when working in Roblox's dialect of Lua.

- Declare all variables as local unless global scope is explicitly required
- Use tabs for indentation, never spaces (matches Roblox Studio default)
- Add comprehensive function documentation with parameter types, return values, and usage examples
- Use type annotations for all function parameters and return types: function(param: number): string
- Prefer task.wait(), task.spawn(), and task.defer() over deprecated wait(), spawn(), delay()
- Use string interpolation with backticks: Player {name} has {health} health
- Implement table.freeze() for immutable configuration data and constants
- Use meaningful variable names that clearly describe their purpose and content
- Follow consistent naming conventions: camelCase for variables, PascalCase for modules
- Do not name things Legacy, always use current technologies and methods
- Do not name things Refactored, always keep it as is when making major changes unless renamed by instruction

## Roblox Studio Workflow

- Organize projects with clear folder structure: ServerStorage/Modules, StarterPlayerScripts/Modules, ReplicatedStorage/Shared
- Cache all service references at the top of modules: local Players = game:GetService("Players")
- Use ContentProvider:PreloadAsync() for assets that need immediate availability
- Enable all relevant Studio debugging features during development
- Structure RemoteEvents and RemoteFunctions in dedicated folders with descriptive names
- Use Attributes for configuration values instead of hardcoded constants
- Implement proper cleanup for temporary objects using Debris:AddItem()
- Test across multiple device types (mobile, console, desktop) regularly

## Rojo Integration & External Development

- Use default.project.json to define clear project structure with logical path mappings
- Organize source code in src/server, src/client, src/shared directories
- Configure VS Code with Luau LSP and proper directory aliases for require paths
- Use consistent require patterns with absolute paths from ReplicatedStorage/ServerStorage
- Create index files (init.luau) for organized module exports and imports
- Implement hot-reloading workflow with rojo serve for rapid development iteration
- Build production files with rojo build and test thoroughly before publishing
- Version control only source files, never built .rbxl or .rbxlx files

## API Usage & Documentation

- Store all RBXScriptConnections for proper cleanup and memory management
- Handle players who joined before scripts loaded using Players:GetPlayers()
- Use proper event connection patterns with error handling and connection storage
- Document all public module APIs with parameter types, return values, and usage examples
- Implement consistent error handling with pcall() for potentially failing operations
- Use appropriate API methods: FindFirstChildOfClass() over generic FindFirstChild()
- Follow official Roblox API documentation patterns and naming conventions
- Cache expensive API calls and avoid repeated property lookups

## Performance Optimization

- Use RunService.Heartbeat for game loops instead of while true do wait() end patterns
- Cache frequently accessed objects and services at module scope
- Implement object pooling for frequently created/destroyed instances
- Batch RemoteEvent calls instead of firing individually for multiple data updates
- Use task.spawn() for non-critical background operations to avoid blocking
- Limit nested table operations and prefer flat data structures where possible
- Use Debris service for automatic cleanup of temporary effects and objects
- Profile code regularly using MicroProfiler and Developer Console performance tools

## Security & Anti-Exploit

- Always validate all client input on the server before processing
- Never trust client-provided data for critical game logic or economy systems
- Implement server-side sanity checks for player positions, stats, and actions
- Use rate limiting for RemoteEvents to prevent spam and abuse
- Sanitize all string inputs and use TextService for chat filtering
- Store sensitive data only on the server, never replicate to clients
- Validate player permissions before allowing administrative actions
- Log suspicious activity and implement automatic detection for impossible actions

## UI/UX Development

- Use Scale (UDim2 first parameter) instead of Offset for responsive design
- Set appropriate AnchorPoint values for proper positioning and scaling
- Implement device-specific layouts for mobile, console, and desktop
- Use TweenService for all animations instead of manual property changes
- Create accessible interfaces with proper gamepad navigation support
- Test UI scaling across different screen resolutions and aspect ratios
- Implement loading states and feedback for all user actions
- Use UIConstraints for automatic sizing and proper text scaling

## Code Architecture & Modularity

- Structure modules with clear public/private method separation using underscore prefixes
- Implement proper constructors with Module.new() and cleanup with :Destroy() methods
- Use dependency injection for testable and modular code architecture
- Create centralized event systems for decoupled communication between systems
- Follow single responsibility principle: one module handles one specific functionality
- Implement proper error boundaries and graceful failure handling
- Use configuration objects passed to constructors instead of global state
- Design modules to be easily testable in isolation from game environment

## Testing & Debugging

- Create unit tests for all utility functions and critical game logic
- Use structured logging with different levels (ERROR, WARN, INFO, DEBUG)
- Implement comprehensive error messages with context and troubleshooting information
- Test edge cases including empty inputs, nil values, and boundary conditions
- Use assertions to validate assumptions and catch bugs during development
- Create mock objects for testing modules that depend on Roblox services
- Test multiplayer scenarios with multiple clients and various network conditions
- Document known issues and limitations clearly for future developers

## Collaboration & Version Control

- Use .gitignore to exclude build artifacts and include only source files
- Follow consistent commit message conventions with clear descriptions
- Create meaningful branch names that describe the feature or fix being implemented
- Code review all changes with focus on performance, security, and maintainability
- Document breaking changes and version updates in changelog format
- Use semantic versioning for module releases and API changes
- Share coding standards and style guides across the development team
- Maintain up-to-date README files with setup instructions and project overview

## Data Management

- Use DataStoreService with proper error handling and retry logic
- Implement data versioning for backward compatibility during updates
- Cache player data locally and sync periodically to reduce DataStore calls
- Use JSON encoding for complex data structures in DataStores
- Implement graceful degradation when DataStore services are unavailable
- Separate player progression data from temporary session data
- Use appropriate DataStore budgets and respect rate limiting
- Create backup systems for critical player data and economy information

## Network Optimization

- Minimize RemoteEvent frequency by batching updates and using local prediction
- Use appropriate data serialization to reduce network payload sizes
- Implement client-side prediction for responsive gameplay feel
- Send only necessary data to clients based on relevance and proximity
- Use RemoteEvents for actions, RemoteFunctions for queries requiring responses
- Implement heartbeat systems for connection monitoring and timeout handling
- Optimize for high latency connections with appropriate buffering strategies
- Consider network partitioning for games with large numbers of concurrent players
