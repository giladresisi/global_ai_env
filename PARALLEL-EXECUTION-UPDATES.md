# Parallel Execution Updates Summary

## Overview

Updated `create-prd.md` and `plan-feature.md` to generate outputs optimized for parallel execution by agent teams.

## Changes to `create-prd.md`

### 1. New Section: Parallel Execution Architecture

Added comprehensive section defining:
- **Team Structure**: Workstream teams with clear responsibilities
- **Interface Contracts**: Formal contracts between teams enabling independent work
- **Shared Context**: Global decisions and conventions all teams must follow
- **Synchronization Points**: Integration checkpoints where teams verify compatibility
- **Dependency Graph**: Visual representation of what can run in parallel

### 2. Enhanced Implementation Phases

Transformed phases from sequential to parallel-ready:
- **Parallelization Strategy**: How many workstreams can run concurrently
- **Workstream Breakdown**: Individual workstreams per phase with:
  - Agent role assignments
  - Clear dependencies
  - Integration points (what they provide/receive)
  - Individual validation commands
- **Phase Integration Tests**: Verify all workstreams integrate correctly

### 3. Updated Requirements Gathering

Added checklist items:
- Workstream boundaries identification
- Interface contracts between teams
- Shared context and dependencies mapping
- Integration checkpoint planning
- Mock/stub strategies for blocked work

### 4. Parallel Execution Planning Questions

New questions to ask during PRD creation:
- Can frontend/backend be built concurrently?
- What shared interfaces need definition first?
- Which components have no dependencies?
- What mock strategies enable parallel development?
- Where are integration synchronization points?

### 5. Enhanced Verification Checklist

Added parallel execution quality checks:
- Parallel execution architecture defined
- Workstreams identified with clear boundaries
- Interface contracts specified
- Workstream boundaries align with architecture
- Parallelization strategy maximizes efficiency
- Integration checkpoints are testable

## Changes to `plan-feature.md`

### 1. New Section: Parallel Execution Strategy

Added before Implementation Plan:
- **Dependency Graph**: Visual wave-based execution diagram
- **Parallelization Opportunities**: Tasks grouped by execution waves
- **Team Coordination**: How agents coordinate without blocking
- **Interface Contracts**: Formal definitions of cross-task dependencies
- **Synchronization Checkpoints**: Integration points after each wave

### 2. New Section: Shared Context Management

Defined shared context structure:
- Global decisions registry
- Naming conventions
- Interface registry with status tracking
- Blockers & questions board
- Integration test results tracker
- Update protocol for agents

### 3. New Section: Agent Team Roles

Defined specializations:
- **Backend Specialist**: API, business logic, database
- **Frontend Specialist**: UI, state, interactions
- **Test Specialist**: All testing layers
- **Integration Specialist**: System integration, deployment
- Coordination protocols between roles

### 4. Enhanced Task Format

Tasks now include parallel execution metadata:
- **WAVE**: Execution wave number
- **AGENT_ROLE**: Suggested agent specialization
- **DEPENDS_ON**: Task IDs that must complete first
- **BLOCKS**: Task IDs waiting for this task
- **PROVIDES**: What this task delivers downstream
- **INTEGRATION_TEST**: How to verify integration with parallel tasks

### 5. Wave-Based Task Organization

Tasks organized into execution waves:
- **Wave 1**: Fully parallel foundation tasks
- **Wave 2**: Parallel tasks after Wave 1
- **Wave 3**: Sequential integration tasks
- Each wave has integration checkpoint

### 6. Design Decisions Update

Added to Phase 2 design considerations:
- Design for parallel execution from the start
- Identify parallelization opportunities
- Define interface contracts
- Map integration points

### 7. Enhanced Final Report

Added parallel execution metrics:
- Number of execution waves
- Tasks that can run concurrently
- Maximum speedup with N agents
- Sequential vs parallel task counts
- Recommended team size

### 8. Enhanced Quality Criteria

New "Parallel Execution Ready" checklist:
- Tasks organized into execution waves
- Dependencies explicitly documented
- Interface contracts defined
- Synchronization checkpoints identified
- Shared context structure documented
- Agent role assignments suggested
- Mock strategies for blocked work
- Integration tests per wave
- At least 30% of tasks parallelizable

### 9. Updated Final Checklist

Added mandatory checks:
- Tasks organized into waves
- Each task has WAVE, DEPENDS_ON, AGENT_ROLE metadata
- Interface contracts defined
- Integration checkpoints specified
- Shared context structure documented
- Minimum 30% parallelization (or justification)

## Key Benefits

### For PRDs:
1. **Clear Team Boundaries**: Teams know exactly what they own
2. **Independent Progress**: Teams can work without blocking each other
3. **Formal Contracts**: Interfaces are defined upfront
4. **Early Integration**: Regular checkpoints catch integration issues early
5. **Scalability**: More teams = faster delivery (up to parallelization limit)

### For Feature Plans:
1. **Wave-Based Execution**: Clear phases of parallel work
2. **Dependency Transparency**: Every task knows what it needs
3. **Role Clarity**: Agents know which tasks match their expertise
4. **Integration Safety**: Tests at each wave prevent big-bang failures
5. **Context Sharing**: Agents coordinate through shared documents
6. **Blocked Work Mitigation**: Mock strategies keep work flowing

## Example Workflow

### PRD Creation:
1. Identify workstreams (e.g., Frontend, Backend, Tests)
2. Define interface contracts (e.g., API schema, data models)
3. Map dependencies (e.g., Backend must finish auth before Frontend login)
4. Plan checkpoints (e.g., After Phase 1, verify all APIs integrate)

### Plan Execution:
1. Create Wave 1 tasks (no dependencies)
2. Assign to specialized agents
3. Agents execute in parallel
4. Run Wave 1 integration checkpoint
5. Create Wave 2 tasks (depend on Wave 1)
6. Repeat until complete

## Migration Guide

### For Existing PRDs:
1. Add "Parallel Execution Architecture" section
2. Break implementation phases into workstreams
3. Define interface contracts between workstreams
4. Add integration checkpoints

### For Existing Plans:
1. Analyze task dependencies
2. Group tasks into waves
3. Add metadata (WAVE, DEPENDS_ON, AGENT_ROLE)
4. Create shared context document
5. Define integration tests per wave

## Metrics to Track

- **Parallelization Ratio**: % of tasks that can run in parallel
- **Wave Count**: Number of sequential waves required
- **Max Speedup**: Theoretical maximum with infinite agents
- **Actual Speedup**: Real speedup with N agents
- **Integration Overhead**: Time spent on checkpoints vs implementation
- **Blocked Time**: Time agents spend waiting for dependencies

## Success Criteria

A well-parallelized PRD/plan should:
- ✅ Enable 3+ workstreams to work independently
- ✅ Have formal interface contracts defined
- ✅ Include integration tests at each wave
- ✅ Achieve 30-50% parallelization ratio
- ✅ Minimize blocking dependencies
- ✅ Support scaling with additional agents
