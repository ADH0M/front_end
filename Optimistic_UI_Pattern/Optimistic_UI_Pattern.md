# Optimistic updates

- is a pattern that helps you update the UI immediately, assuming the server operation will succeed,
  and if it later fails, you roll back the UI to the correct state.

- Instead of waiting for the round-trip of the client request, database write, server response,
  and then the UI render, the UI just updates instantly.

- This dramatically increases what’s called the perceived speed,The user of the application perceives
  the UI update as instant – but the actual operation may take place in the background.

## Without an Optimistic Update

- If you’re not using the optimistic pattern, it’s just a traditional client-server mechanism, where:

  > - At the client side, a user interacts with a UI element.
  > - An async call (request) is made to the server.
  > - The server processes the request and may make DB updates.
  > - On a successful case, the server sends back the response to the client.
  > - The client updates the relevant UI.
  > - In an error case, the server sends back the error response to the client.
  > - The client informs the user about the error.

  ```ts
                    wait
    user clients  =======>   server request
                                    ||
                    -------------------------------------
                    |                                   |
                server success                    server fialure
                (ui update)                        (inform user)

  ```

## With an Optimistic Update

```tsx
    user client  ============> ui updates optimisticaly
                |
                |in parallel background
                ==========================> server request
                                                  ||
                                ----------------------------------------
                                |                                       |
                                server success                    server failure
                                Do nothing                        rollback ui
```

## we can now understand why Optimistic Updates matter in modern UI

- it improves the perceived speed
- it keeps user engagement
- it eliminates the awkward feelings like **_Did my click work?_**
- optimistic updates are critical for real time apps [chat messages , cart updates , poll votes , ]

## how does it work under the hood?

- Under the hood, there are actually two states
  > - The Actual State: This is the actual source of truth ,This data should be in sync with the server.
  > - The Optimistci State: This is temporary and instantly shown to the user.
- When the server request succeeds, do nothing. Your optimistic state is now correct.
  If the server request fails, perform a rollback, and return UI the actual state.

```ts
const [optimisticState, addOptimistic] = useOptimistic(state, updateFn);
```

> When an async action is underway, the useOptimistic() hook allows you to show different states.

**_It accepts_**:

- **currentState** : your real source of truth (useState, Redux, server state, and so on)
- **updateFn** : a pure function that says how to compute the optimistic value

**_It return_**

- **optimisticState** : the temporary UI state
- **addOptimisticUpdate(input)** : function you call to apply immediate updates

## Example

```tsx
import { startTransition, useOptimistic, useState } from "react";

// simulate a network call to the Server
async function sendLikeToServer(postId) {
  await new Promise((r) => setTimeout(r, 700));

  if (Math.random() < 0.2) throw new Error("Network failed");
  console.log(`Sent a like for the post id ${postId}`);
  return { success: true };
}

// The Like Button Component
export default function LikeButton({ postId, initialLikes = 0 }) {
  // the "real" source of truth for likes (committed)
  const [likes, setLikes] = useState(initialLikes);
  // optimistic state and updater function
  const [optimisticLikes, addOptimisticLike] = useOptimistic(
    likes,
    (currentLikes, delta) => currentLikes + delta,
  );

  const handleLike = async () => {
    // 1) Apply optimistic change *immediately*
    addOptimisticLike(1);

    // 2) Start server call in low priority to avoid blocking UI

    try {
      await sendLikeToServer(postId);
      // On success, commit the real state update:
      // IMPORTANT: update the real state so optimistic snapshot eventually matches
      setLikes((prev) => prev + 1);
    } catch (err) {
      // On error, rollback the real state (or trigger a refetch)
      // Because we never incremented likes (real), just leave likes unchanged
      // But we should show an error to user:
      console.error("Like failed:", err);
      // Optionally: show toast or set an error state
      // And — to force the optimistic view to refresh and reflect real state,
      // call setLikes to current value
      setLikes((s) => s); // no-op but will cause optimistic to reflect the
      // committed value Or you can trigger a re-fetch of the
      // post state
    }
  };

  return (
    <div className="flex">
      <button onClick={handleLike}>❤️ {optimisticLikes}</button>
      <button onClick={() => startTransition(async () => handleLike())}>
        ❤️ {optimisticLikes}
      </button>
    </div>
  );
}
```

<!--
## some english keywords
> - roll back ----------------------------- الرجوع
> - optimistic
> - instantly -instant
> - take place ----------------------------- تحصل او تتم او تحدث
> - perceived speed
> - what's celled
> - relevant ----------------------------- ذات الصلة
> - inform ----------------------------- يبلغ -يخبر
> - perceive ----------------------------- اِسْتَشْعَرَ- يدرك -يتصور
> - the eager ----------------------------- المتحمسون
> - user engagement
> - awkward ----------------------------- محرج
> - the Actual source of truth
> - temporary
> - we have been كنا
> - we have been able to استطعنا
> - underway قيد التنفيذ -->
