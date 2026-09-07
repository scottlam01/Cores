# Cores
> [!NOTE]
> This repository documents my individual contributions to a collaborative academic project. The original source code is not publicly available due to course/project restrictions.

## Play the Game
cores-frontend.vercel.app

## Project Overview
The game, Cores, allows players to create profiles, join game lobbies, select decks, and participate in real-time card battles. It takes the complex features of traditional trading card games and turns it less complex and simple.  The frontend was built with React and TypeScript, with Supabase used for authentication and backend services. The application also includes a custom game engine responsible for managing game state, player actions, and gameplay logic.

## Personal Contributions
- Implementing authentication and user profile functionality using React and Supabase
- Developing the game lobby and integrating the flow from the home screen to the battleground
- Implementing gameplay functionality, including the mulligan system
- Developing and improving battleground UI features such as player avatars, game logs, and hand-size displays
- Debugging and fixing gameplay and UI issues throughout development

## Technologies
- Frontend: React, TypeScript, CSS
- Backend / Services: Supabase
- Game Engine: TypeScript
- Authentication: Supabase Auth
- Version Control: Git, GitHub

## Screenshots
<table>
  <tr>
    <td><img width="1920" height="1079" alt="tutorial_slide4" src="https://github.com/user-attachments/assets/a95fb9d8-bc39-43fe-85b4-d0ce43d4ed05" /></td>
    <td><img width="1920" height="1079" alt="tutorial_slide6" src="https://github.com/user-attachments/assets/77b88e58-4da1-4c69-bfe3-5b059f8a2750" /></td>
  </tr>
  <tr>
    <td><img width="1920" height="1079" alt="tutorial_slide8" src="https://github.com/user-attachments/assets/c4f8d276-fd12-4e19-88df-89246b2c9e82" /></td>
    <td><img width="1920" height="1079" alt="tutorial_slide10" src="https://github.com/user-attachments/assets/8ccb1783-0cb5-45b6-89ca-d568b0a016f1" /></td>
  </tr>
</table>


## Code Snippets
### Authentication State Management
The authentication context manages the user's Supabase session throughout the React application. It initializes the current session, listens for authentication changes, and cleans up the authentication subscription when the provider is unmounted.
```tsx
// On mount, get the current session and subscribe to auth state changes
useEffect(() => {
  supabase.auth.getSession().then(({ data: { session } }) => {
    setSession(session);
  });
  const {
    data: { subscription },
  } = supabase.auth.onAuthStateChange((_event, session) => {
    setSession(session);
  });
  return () => subscription.unsubscribe();
});
```
### Mulligan Feature
The implementation validates whether a player can mulligan, handles both skipping and replacing a card, updates the player's game state, draws a replacement card, and records the action in the game log.
```tsx
case 'MULLIGAN': {
  const { playerId, data } = action;
  const { cardInstanceId, skip } = data;

  const player =
    playerId === newGameState.player1.id
      ? newGameState.player1
      : newGameState.player2;

  if (!player.canMulligan) {
    logs.push('Mulligan already used.');
    break;
  }

  if (skip) {
    player.canMulligan = false;
    logs.push(`${player.username} skipped their mulligan.`);
  } else {
    const card = this.removeCardFromHand(
      newGameState,
      player.id,
      cardInstanceId
    );

    if (card) {
      this.newEngine.drawCards(newGameState, player.id, 1);
      player.canMulligan = false;

      logs.push(
        `${player.username} mulligans "${card.name}" and draws a new card.`
      );
    }
  }

  result = newGameState;
  break;
}
```
