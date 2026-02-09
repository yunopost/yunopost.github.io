<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Board Game Night Organizer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        h1 {
            text-align: center;
            color: white;
            margin-bottom: 30px;
            font-size: 2.5em;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }
        
        .section {
            background: white;
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.1);
        }
        
        .section h2 {
            color: #667eea;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #667eea;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            color: #333;
            font-weight: 500;
        }
        
        input[type="text"],
        input[type="number"],
        input[type="date"],
        select {
            width: 100%;
            padding: 10px;
            border: 2px solid #e0e0e0;
            border-radius: 6px;
            font-size: 14px;
            transition: border-color 0.3s;
        }
        
        input[type="text"]:focus,
        input[type="number"]:focus,
        input[type="date"]:focus,
        select:focus {
            outline: none;
            border-color: #667eea;
        }
        
        input[type="checkbox"] {
            margin-right: 8px;
            width: 18px;
            height: 18px;
            cursor: pointer;
        }
        
        .checkbox-label {
            display: flex;
            align-items: center;
            cursor: pointer;
        }
        
        button {
            background: #667eea;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 6px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        button:hover {
            background: #5568d3;
        }
        
        button.danger {
            background: #e74c3c;
        }
        
        button.danger:hover {
            background: #c0392b;
        }
        
        button.secondary {
            background: #95a5a6;
        }
        
        button.secondary:hover {
            background: #7f8c8d;
        }
        
        .game-list {
            display: grid;
            gap: 15px;
        }
        
        .game-item {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
        }
        
        .game-item h3 {
            color: #333;
            margin-bottom: 8px;
        }
        
        .game-detail {
            color: #666;
            font-size: 14px;
            margin: 4px 0;
        }
        
        .rank-selector {
            display: grid;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .rank-item {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .rank-item label {
            min-width: 100px;
            margin: 0;
        }
        
        .rank-item select {
            flex: 1;
        }
        
        .winner-section {
            background: #fff9e6;
            padding: 20px;
            border-radius: 8px;
            border: 2px solid #ffd700;
            margin-top: 20px;
        }
        
        .winner-section h3 {
            color: #f39c12;
            margin-bottom: 15px;
        }
        
        .player-list {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 8px;
        }
        
        .player-badge {
            background: #667eea;
            color: white;
            padding: 4px 12px;
            border-radius: 12px;
            font-size: 13px;
        }
        
        .history-item {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
        }
        
        .history-item h4 {
            color: #667eea;
            margin-bottom: 10px;
        }
        
        .admin-controls {
            display: grid;
            gap: 15px;
        }
        
        .voting-status {
            padding: 10px;
            border-radius: 6px;
            text-align: center;
            font-weight: 600;
            margin-bottom: 15px;
        }
        
        .voting-open {
            background: #d4edda;
            color: #155724;
        }
        
        .voting-closed {
            background: #f8d7da;
            color: #721c24;
        }
        
        .warning {
            background: #fff3cd;
            border: 1px solid #ffc107;
            color: #856404;
            padding: 12px;
            border-radius: 6px;
            margin-bottom: 15px;
        }
        
        .overflow-indicator {
            color: #e74c3c;
            font-size: 13px;
            font-style: italic;
            margin-top: 4px;
        }
        
        .two-column {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
        
        @media (max-width: 768px) {
            .two-column {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎲 Board Game Night Organizer</h1>
        
        <!-- Admin Controls -->
        <div class="section">
            <h2>⚙️ Admin Controls</h2>
            <div class="admin-controls">
                <div class="form-group">
                    <label for="votingDeadline">Voting Deadline:</label>
                    <input type="date" id="votingDeadline">
                </div>
                <button onclick="saveVotingDeadline()">Set Voting Deadline</button>
                <button class="danger" onclick="resetForNewWeek()">Reset for New Week</button>
            </div>
            <div id="votingStatus" class="voting-status"></div>
        </div>
        
        <!-- Add Games -->
        <div class="section">
            <h2>➕ Add a Game</h2>
            <div class="form-group">
                <label for="ownerName">Your Name:</label>
                <input type="text" id="ownerName" placeholder="Enter your name">
            </div>
            <div class="form-group">
                <label for="gameName">Game Name:</label>
                <input type="text" id="gameName" placeholder="Enter game name">
            </div>
            <div class="two-column">
                <div class="form-group">
                    <label for="minPlayers">Min Players:</label>
                    <input type="number" id="minPlayers" min="1" value="2">
                </div>
                <div class="form-group">
                    <label for="maxPlayers">Max Players:</label>
                    <input type="number" id="maxPlayers" min="1" value="4">
                </div>
            </div>
            <div class="form-group">
                <label class="checkbox-label">
                    <input type="checkbox" id="canTeach">
                    I can teach this game
                </label>
            </div>
            <button onclick="addGame()">Add Game</button>
            
            <div id="gamesList" class="game-list" style="margin-top: 20px;"></div>
        </div>
        
        <!-- Voting -->
        <div class="section">
            <h2>🗳️ Vote for This Week's Games</h2>
            <div id="votingWarning" class="warning" style="display: none;"></div>
            <div class="form-group">
                <label for="voterName">Your Name:</label>
                <input type="text" id="voterName" placeholder="Enter your name">
            </div>
            <div class="rank-selector">
                <div class="rank-item">
                    <label for="choice1">1st Choice:</label>
                    <select id="choice1">
                        <option value="">Select a game</option>
                    </select>
                </div>
                <div class="rank-item">
                    <label for="choice2">2nd Choice:</label>
                    <select id="choice2">
                        <option value="">Select a game</option>
                    </select>
                </div>
                <div class="rank-item">
                    <label for="choice3">3rd Choice:</label>
                    <select id="choice3">
                        <option value="">Select a game</option>
                    </select>
                </div>
            </div>
            <button onclick="submitVote()">Submit Vote</button>
        </div>
        
        <!-- This Week's Games -->
        <div class="section">
            <h2>🏆 This Week's Top Games</h2>
            <div id="topGames"></div>
        </div>
        
        <!-- History -->
        <div class="section">
            <h2>📜 Game Night History</h2>
            <div class="winner-section">
                <h3>Record Winner for Current Week</h3>
                <div class="form-group">
                    <label for="winnerGame">Select Game:</label>
                    <select id="winnerGame">
                        <option value="">Select a game</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="winnerName">Winner Name:</label>
                    <input type="text" id="winnerName" placeholder="Enter winner's name">
                </div>
                <div class="form-group">
                    <label for="winnerDate">Date Played:</label>
                    <input type="date" id="winnerDate">
                </div>
                <button onclick="recordWinner()">Record Winner</button>
            </div>
            
            <div id="historyList" style="margin-top: 20px;"></div>
        </div>
    </div>

    <script>
        // Initialize data structure
        let gameData = {
            games: [],
            votes: [],
            history: [],
            votingDeadline: null
        };

        // Load data on startup
        async function loadData() {
            try {
                const result = await window.storage.get('boardgame-data');
                if (result && result.value) {
                    gameData = JSON.parse(result.value);
                }
            } catch (error) {
                console.log('No existing data found, starting fresh');
            }
            updateAllDisplays();
        }

        // Save data
        async function saveData() {
            try {
                await window.storage.set('boardgame-data', JSON.stringify(gameData));
            } catch (error) {
                console.error('Error saving data:', error);
                alert('Error saving data. Please try again.');
            }
        }

        // Check if voting is open
        function isVotingOpen() {
            if (!gameData.votingDeadline) return true;
            const deadline = new Date(gameData.votingDeadline);
            const now = new Date();
            return now < deadline;
        }

        // Update voting status display
        function updateVotingStatus() {
            const statusDiv = document.getElementById('votingStatus');
            const warningDiv = document.getElementById('votingWarning');
            
            if (!gameData.votingDeadline) {
                statusDiv.className = 'voting-status voting-open';
                statusDiv.textContent = 'Voting is OPEN (no deadline set)';
                warningDiv.style.display = 'none';
            } else {
                const deadline = new Date(gameData.votingDeadline);
                const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
                const dateStr = deadline.toLocaleDateString('en-US', options);
                
                if (isVotingOpen()) {
                    statusDiv.className = 'voting-status voting-open';
                    statusDiv.textContent = `Voting is OPEN until ${dateStr}`;
                    warningDiv.style.display = 'none';
                } else {
                    statusDiv.className = 'voting-status voting-closed';
                    statusDiv.textContent = `Voting CLOSED on ${dateStr}`;
                    warningDiv.style.display = 'block';
                    warningDiv.textContent = 'Voting is currently closed. Contact the organizer to reopen voting.';
                }
            }
        }

        // Save voting deadline
        function saveVotingDeadline() {
            const deadline = document.getElementById('votingDeadline').value;
            if (!deadline) {
                alert('Please select a deadline date');
                return;
            }
            gameData.votingDeadline = deadline;
            saveData();
            updateVotingStatus();
            alert('Voting deadline set successfully!');
        }

        // Add a game
        function addGame() {
            const ownerName = document.getElementById('ownerName').value.trim();
            const gameName = document.getElementById('gameName').value.trim();
            const minPlayers = parseInt(document.getElementById('minPlayers').value);
            const maxPlayers = parseInt(document.getElementById('maxPlayers').value);
            const canTeach = document.getElementById('canTeach').checked;

            if (!ownerName || !gameName) {
                alert('Please enter both your name and game name');
                return;
            }

            if (minPlayers > maxPlayers) {
                alert('Min players cannot be greater than max players');
                return;
            }

            // Check if game already exists
            const existingGame = gameData.games.find(g => g.name.toLowerCase() === gameName.toLowerCase());
            
            if (existingGame) {
                // Add owner to existing game
                if (!existingGame.owners.find(o => o.name.toLowerCase() === ownerName.toLowerCase())) {
                    existingGame.owners.push({ name: ownerName, canTeach: canTeach });
                    alert(`Added ${ownerName} as an owner of ${gameName}`);
                } else {
                    alert(`${ownerName} is already listed as an owner of ${gameName}`);
                    return;
                }
            } else {
                // Create new game
                const game = {
                    id: Date.now(),
                    name: gameName,
                    owners: [{ name: ownerName, canTeach: canTeach }],
                    minPlayers: minPlayers,
                    maxPlayers: maxPlayers
                };
                gameData.games.push(game);
            }

            // Clear form
            document.getElementById('ownerName').value = '';
            document.getElementById('gameName').value = '';
            document.getElementById('minPlayers').value = '2';
            document.getElementById('maxPlayers').value = '4';
            document.getElementById('canTeach').checked = false;

            saveData();
            updateAllDisplays();
        }

        // Display games list
        function displayGames() {
            const gamesDiv = document.getElementById('gamesList');
            
            if (gameData.games.length === 0) {
                gamesDiv.innerHTML = '<p style="color: #999;">No games added yet</p>';
                return;
            }

            gamesDiv.innerHTML = gameData.games.map(game => {
                const ownersList = game.owners.map(o => 
                    `${o.name}${o.canTeach ? ' (can teach)' : ''}`
                ).join(', ');

                return `
                    <div class="game-item">
                        <h3>${game.name}</h3>
                        <div class="game-detail">Brought by: ${ownersList}</div>
                        <div class="game-detail">Players: ${game.minPlayers}-${game.maxPlayers}</div>
                    </div>
                `;
            }).join('');
        }

        // Update vote dropdowns
        function updateVoteDropdowns() {
            const selects = [
                document.getElementById('choice1'),
                document.getElementById('choice2'),
                document.getElementById('choice3')
            ];

            selects.forEach(select => {
                const currentValue = select.value;
                select.innerHTML = '<option value="">Select a game</option>';
                
                gameData.games.forEach(game => {
                    const option = document.createElement('option');
                    option.value = game.id;
                    option.textContent = game.name;
                    select.appendChild(option);
                });
                
                select.value = currentValue;
            });

            // Update winner dropdown
            const winnerSelect = document.getElementById('winnerGame');
            const currentWinnerValue = winnerSelect.value;
            winnerSelect.innerHTML = '<option value="">Select a game</option>';
            
            const topGames = calculateTopGames();
            topGames.forEach(gameResult => {
                const option = document.createElement('option');
                option.value = gameResult.game.id;
                option.textContent = gameResult.game.name;
                winnerSelect.appendChild(option);
            });
            
            winnerSelect.value = currentWinnerValue;
        }

        // Submit vote
        function submitVote() {
            if (!isVotingOpen()) {
                alert('Voting is currently closed!');
                return;
            }

            const voterName = document.getElementById('voterName').value.trim();
            const choice1 = document.getElementById('choice1').value;
            const choice2 = document.getElementById('choice2').value;
            const choice3 = document.getElementById('choice3').value;

            if (!voterName) {
                alert('Please enter your name');
                return;
            }

            if (!choice1 || !choice2 || !choice3) {
                alert('Please select all 3 choices');
                return;
            }

            if (choice1 === choice2 || choice1 === choice3 || choice2 === choice3) {
                alert('Please select 3 different games');
                return;
            }

            // Remove existing vote from this person
            gameData.votes = gameData.votes.filter(v => v.voterName.toLowerCase() !== voterName.toLowerCase());

            // Add new vote
            const vote = {
                voterName: voterName,
                choices: [
                    parseInt(choice1),
                    parseInt(choice2),
                    parseInt(choice3)
                ]
            };

            gameData.votes.push(vote);

            // Clear form
            document.getElementById('voterName').value = '';
            document.getElementById('choice1').value = '';
            document.getElementById('choice2').value = '';
            document.getElementById('choice3').value = '';

            saveData();
            updateAllDisplays();
            alert('Vote submitted successfully!');
        }

        // Calculate top games with spillover
        function calculateTopGames() {
            if (gameData.games.length === 0 || gameData.votes.length === 0) {
                return [];
            }

            // Count votes using ranked choice
            const gameScores = {};
            gameData.games.forEach(game => {
                gameScores[game.id] = { score: 0, voters: [] };
            });

            gameData.votes.forEach(vote => {
                vote.choices.forEach((gameId, index) => {
                    const points = [3, 2, 1][index]; // 1st choice = 3 points, 2nd = 2, 3rd = 1
                    gameScores[gameId].score += points;
                    gameScores[gameId].voters.push({
                        name: vote.voterName,
                        rank: index + 1,
                        choices: vote.choices
                    });
                });
            });

            // Sort by score
            const sortedGames = Object.entries(gameScores)
                .map(([gameId, data]) => ({
                    game: gameData.games.find(g => g.id === parseInt(gameId)),
                    score: data.score,
                    voters: data.voters
                }))
                .filter(item => item.game && item.score > 0)
                .sort((a, b) => b.score - a.score);

            // Get top 3
            const top3 = sortedGames.slice(0, 3);

            // Apply spillover logic
            top3.forEach(gameResult => {
                const assignedPlayers = [];
                const overflowPlayers = [];

                // Sort voters by rank (1st choice voters get priority)
                const sortedVoters = [...gameResult.voters].sort((a, b) => a.rank - b.rank);

                sortedVoters.forEach(voter => {
                    if (assignedPlayers.length < gameResult.game.maxPlayers) {
                        assignedPlayers.push(voter.name);
                    } else {
                        // Find their next choice that's in top 3 and not full
                        let assigned = false;
                        for (let i = voter.rank; i < 3; i++) {
                            const nextChoiceId = voter.choices[i];
                            const nextGame = top3.find(g => g.game.id === nextChoiceId);
                            
                            if (nextGame && nextGame !== gameResult) {
                                if (!nextGame.assignedPlayers) nextGame.assignedPlayers = [];
                                if (!nextGame.overflowPlayers) nextGame.overflowPlayers = [];
                                
                                if (nextGame.assignedPlayers.length < nextGame.game.maxPlayers &&
                                    !nextGame.assignedPlayers.includes(voter.name)) {
                                    nextGame.assignedPlayers.push(voter.name);
                                    assigned = true;
                                    overflowPlayers.push(`${voter.name} (moved to ${nextGame.game.name})`);
                                    break;
                                }
                            }
                        }
                        if (!assigned) {
                            overflowPlayers.push(`${voter.name} (no available slot)`);
                        }
                    }
                });

                gameResult.assignedPlayers = assignedPlayers;
                gameResult.overflowPlayers = overflowPlayers;
            });

            return top3;
        }

        // Display top games
        function displayTopGames() {
            const topGamesDiv = document.getElementById('topGames');
            const topGames = calculateTopGames();

            if (topGames.length === 0) {
                topGamesDiv.innerHTML = '<p style="color: #999;">No votes yet</p>';
                return;
            }

            topGamesDiv.innerHTML = topGames.map((gameResult, index) => {
                const ownersList = gameResult.game.owners.map(o => 
                    `${o.name}${o.canTeach ? ' 👨‍🏫' : ''}`
                ).join(', ');

                const players = gameResult.assignedPlayers || [];
                const overflow = gameResult.overflowPlayers || [];

                return `
                    <div class="game-item" style="border-left-color: ${['#ffd700', '#c0c0c0', '#cd7f32'][index]};">
                        <h3>${index + 1}. ${gameResult.game.name} (${gameResult.score} points)</h3>
                        <div class="game-detail">Brought by: ${ownersList}</div>
                        <div class="game-detail">Players: ${gameResult.game.minPlayers}-${gameResult.game.maxPlayers}</div>
                        <div class="game-detail">
                            <strong>Playing (${players.length}/${gameResult.game.maxPlayers}):</strong>
                            <div class="player-list">
                                ${players.map(p => `<span class="player-badge">${p}</span>`).join('')}
                            </div>
                        </div>
                        ${overflow.length > 0 ? `
                            <div class="overflow-indicator">
                                Spillover: ${overflow.join(', ')}
                            </div>
                        ` : ''}
                    </div>
                `;
            }).join('');
        }

        // Record winner
        function recordWinner() {
            const gameId = document.getElementById('winnerGame').value;
            const winnerName = document.getElementById('winnerName').value.trim();
            const winnerDate = document.getElementById('winnerDate').value;

            if (!gameId || !winnerName || !winnerDate) {
                alert('Please fill in all fields');
                return;
            }

            const game = gameData.games.find(g => g.id === parseInt(gameId));
            if (!game) {
                alert('Game not found');
                return;
            }

            const historyEntry = {
                id: Date.now(),
                gameName: game.name,
                winner: winnerName,
                date: winnerDate
            };

            gameData.history.push(historyEntry);

            // Clear form
            document.getElementById('winnerGame').value = '';
            document.getElementById('winnerName').value = '';
            document.getElementById('winnerDate').value = '';

            saveData();
            displayHistory();
            alert('Winner recorded successfully!');
        }

        // Display history
        function displayHistory() {
            const historyDiv = document.getElementById('historyList');
            
            if (gameData.history.length === 0) {
                historyDiv.innerHTML = '<p style="color: #999;">No game history yet</p>';
                return;
            }

            // Sort by date descending
            const sortedHistory = [...gameData.history].sort((a, b) => 
                new Date(b.date) - new Date(a.date)
            );

            historyDiv.innerHTML = sortedHistory.map(entry => {
                const date = new Date(entry.date);
                const dateStr = date.toLocaleDateString('en-US', { 
                    weekday: 'long', 
                    year: 'numeric', 
                    month: 'long', 
                    day: 'numeric' 
                });

                return `
                    <div class="history-item">
                        <h4>🏆 ${entry.gameName}</h4>
                        <div class="game-detail">Winner: <strong>${entry.winner}</strong></div>
                        <div class="game-detail">Date: ${dateStr}</div>
                    </div>
                `;
            }).join('');
        }

        // Reset for new week
        function resetForNewWeek() {
            if (!confirm('This will clear all games and votes for the new week. History will be preserved. Continue?')) {
                return;
            }

            gameData.games = [];
            gameData.votes = [];
            gameData.votingDeadline = null;

            saveData();
            updateAllDisplays();
            alert('Reset complete! Ready for a new week.');
        }

        // Update all displays
        function updateAllDisplays() {
            displayGames();
            updateVoteDropdowns();
            displayTopGames();
            displayHistory();
            updateVotingStatus();
            
            // Set today's date as default for winner recording
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('winnerDate').value = today;
        }

        // Initialize on load
        window.addEventListener('load', () => {
            loadData();
        });
    </script>
</body>
</html>
