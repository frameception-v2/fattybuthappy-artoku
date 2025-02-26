### Step 1: Implement Core Game State Structure
```text
- Build: GameState interface with puzzle/solution matrices, userBoard, artUrl, and status. Basic Redis storage methods (get/set)
- Outcome: Test storing/retrieving game state via redis-cli with sample puzzle:
  SET 123:abc '{"puzzle":[[...]], "solution":[[...]], "userBoard":[[...]], "artUrl":"ipfs://Qm...", "status":"active"}'
  GET 123:abc
```

### Step 2: Create Basic Move Validation Endpoint
```text
- Build: POST /game handler that:
  1. Parses "A5 3" format inputs
  2. Validates against puzzle (immutable cells)
  3. Checks against solution
  4. Updates userBoard
- Outcome: Use curl to send moves and verify updated userBoard:
  curl -X POST -d "A5=3" http://localhost:3000/game?fid=123&session=abc
  Check Redis state for updated cell
```

### Step 3: Develop Basic Image Generation Endpoint
```text
- Build: GET /image/{fid}/{session} that:
  1. Creates 600x600 canvas
  2. Draws Sudoku grid
  3. Renders userBoard numbers
- Outcome: Access endpoint in browser, see valid Sudoku grid with numbers. Verify cell positions match A1-I9 coordinate system.
```

### Step 4: Integrate Custom Art Backgrounds
```text
- Build: Image composition layer that:
  1. Fetches artUrl from GameState
  2. Draws background image first
  3. Overlays Sudoku numbers
- Outcome: Test with 2 different IPFS artworks. Verify numbers remain legible on varied backgrounds.
```

### Step 5: Implement Landing Frame (Screen 1)
```text
- Build: Initial frame with:
  1. Background image from artUrl
  2. Start game button (post to /game?action=start)
  3. Session ID generation logic
- Outcome: Clicking "Start" creates new GameState in Redis and redirects to Screen 2. Verify session IDs increment correctly.
```

### Step 6: Build Game Interface Frame (Screen 2)
```text
- Build: Interactive frame with:
  1. Dynamic /image endpoint as main visual
  2. Text input field for moves
  3. Submit button (post to /game?action=move)
- Outcome: Submitting valid moves updates displayed image. Test consecutive moves persist across reloads.
```

### Step 7: Add Input Validation Feedback
```text
- Build: Error handling for:
  1. Malformed input ("A0 10", "B2 x")
  2. Immutable cell attempts
  3. Incorrect solutions
- Outcome: Test invalid inputs return human-readable errors in frame message (e.g., "Invalid format: Use 'A5 3' syntax").
```

### Step 8: Implement Win Detection (Screen 3)
```text
- Build: Victory check comparing userBoard to solution. Celebration screen with:
  1. Confetti animation
  2. "Puzzle Solved!" message
  3. Play Again button
- Outcome: Solve test puzzle manually via API calls, verify automatic redirect to victory screen.
```

### Step 9: Add Session Validation
```text
- Build: Timestamp comparison using frame message:
  1. Reject messages >15min old
  2. Handle state mismatch by clearing session
- Outcome: Test with modified timestamps - old messages should trigger new game creation.
```

### Step 10: Implement Image Caching
```text
- Build: Content-based hashing system:
  1. Hash calculation from artUrl + userBoard
  2. Redis cache with hash keys
  3. Cache TTL of 1 hour
- Outcome: Verify identical board states return same image URL. Monitor Redis memory usage.
```

### Step 11: Pre-generate Initial Composites
```text
- Build: Background worker that:
  1. Creates puzzle+art composite at game start
  2. Stores pre-rendered base image
  3. Only re-renders number layer
- Outcome: Measure image generation time reduction using console.time() benchmarks.
```

### Step 12: Add Fallback Image Handling
```text
- Build: Error recovery for:
  1. Missing artUrl
  2. Failed IPFS fetches
  3. Invalid image formats
- Outcome: Test with broken art URLs - should display default Sudoku template with warning message in console.
```