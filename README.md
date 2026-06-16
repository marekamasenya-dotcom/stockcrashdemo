# StockCrash — Crash Game

A fully featured trading-themed crash game built for white-label licensing to casino operators.

## Files
- `index.html` — Complete standalone game (no dependencies, no build step required)

## Features

### Gameplay
- Provably fair crash point generation via SHA-256 hash chain
- Smooth bezier multiplier curve with glow tip dot
- 5-second betting window with live countdown
- Auto cashout — set your target and walk away
- Spacebar shortcut for quick cashout
- Milestone particles (2x, 3x, 5x, 10x, 20x...)

### Audio (Web Audio API — no files needed)
- Countdown beep
- Flying ambience tick
- Cashout success chord
- Crash explosion
- Milestone chime
- Bet placement click

### Multiplayer Simulation
- 6 configurable bot players with unique names and avatars
- Live bets panel shows real-time cashouts and busts
- Bot wager sizing randomised each round

### Admin Panel (password-protect before deployment)
- House edge configuration (%)
- Min/max bet limits
- Betting window duration
- Bot player count
- Max multiplier cap
- Live metrics: house profit, total wagered, rounds run
- Hash chain history viewer

### Provably Fair
- SHA-256 hash chain seeded at session start
- Client seed displayed for transparency
- Post-round verification tool built in
- Any player can verify crash point was not manipulated

### Session Stats
- Total wagered, net P&L, biggest win
- Win rate, rounds played, average cashout multiplier

## Integration (for operators)

### Real money wallet
Replace `bal` variable with API calls to your wallet system:
```js
// On bet placed
await api.deductBalance(userId, betAmt);

// On cashout
await api.creditBalance(userId, betAmt * coMult);
```

### Real multiplayer (WebSocket)
Replace bot simulation with a WebSocket server:
```js
const ws = new WebSocket('wss://your-server/game');
ws.onmessage = (e) => {
  const { type, data } = JSON.parse(e.data);
  if (type === 'BET') addLiveBet(data);
  if (type === 'CASHOUT') updateLiveBet(data);
  if (type === 'CRASH') handleCrash(data.point);
};
```

### Server-side crash point
For production, generate crash points server-side and only reveal the seed after the round:
```js
// Server generates before round starts
const serverSeed = crypto.randomBytes(32).toString('hex');
const hash = sha256(serverSeed);
// Send hash to client BEFORE round
// After round ends, reveal serverSeed so client can verify
```

## Customisation
- Edit CSS `:root` variables to retheme colours
- Change `CFG` defaults for house edge and limits
- Replace ticker symbols with your preferred assets
- Add your logo to `#logo-text`

## Licensing
This is a demo/source product for operator licensing. You must hold a valid gaming licence to operate for real money in your jurisdiction.
