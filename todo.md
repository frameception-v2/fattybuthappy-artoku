#### Setup
- [ ] Task 1: Implement Core Game State Structure  
  - Set up GameState interface with puzzle, solution, userBoard, artUrl, status. Basic Redis storage methods (get/set)

#### Core Features
- [ ] Task 2: Create Basic Move Validation Endpoint  
  - Build POST `/game` handler for moves with parsing, validation, and state updates  
  - Depends on: Task 1
- [ ] Task 3: Add Input Validation Feedback  
  - Handle malformed inputs, immutable cells, and incorrect solutions  
  - Depends on: Task 2
- [ ] Task 4: Implement Win Detection  
  - Add victory check and celebration screen logic  
  - Depends on: Task 2
- [ ] Task 7: Implement Session Validation  
  - Add timestamp checks and state mismatch handling  
  - Depends on: Task 8 (UI/UX Landing Frame)

#### API Integration
- [ ] Task 5: Develop Basic Image Generation Endpoint  
  - Build GET `/image` endpoint to render Sudoku grid  
  - Depends on: Task 1
- [ ] Task 9: Implement Image Caching  
  - Add content-based hashing and Redis caching  
  - Depends on: Task 5
- [ ] Task 11: Pre-generate Initial Composites  
  - Create background worker for pre-rendered base images  
  - Depends on: Task 5, Task 10
- [ ] Task 12: Add Fallback Image Handling  
  - Handle missing/invalid art with default templates  
  - Depends