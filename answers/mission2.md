# Mission 2: Console attack, sabotage the purge button

## Evidence

The button dodges (two positions), with my attacker counter visible:

![position 1](img/m2-pos1.png)
![position 2](img/m2-pos2.png)

A legitimate click does nothing after my attack (log still reads "No purge requested"):

![click does nothing](img/m2-click.png)

## My attack script

Paste the full contents of `attacks/m2_runaway.js`, with one sentence per block:

```js
(() => { const zone = document.getElementById("danger-zone");
   const original = document.getElementById("purge-btn"); 
   //TODO R1: remove the portal's legitimate click listener. 
   // Remove the portal's legitimate click handler, so a normal click never purges. 
   // Use the clone technique, do not reload the page. 
   if (zone === null || original === null) { 
    console.error("Could not find #danger-zone or #purge-btn."); 
    return; 
  } 
  const button = original.cloneNode(true); 
  original.replaceWith(button); 
  // Make #danger-zone the positioning container for the button. 
  zone.style.position = "relative"; 
  // TODO R2: stop keyboard users from reaching the button. 
  // Block keyboard users too. A real operator might Tab to the button and press Enter. Stop that.
  // I could make an server message but I thought it would be too much. 
  window.addEventListener("keydown", (event) => { 
    if (event.key === "Tab") {
       event.preventDefault(); 
      } 
    }); 
    // TODO R3: make the button jump inside zone on every approach, no overlap.
    // Make the button jump to a random position, fully inside #danger-zone, whenever
    // the pointer approaches, and never overlap its previous spot.
    // TODO R4: Also count number of dodges with an additional UI. 
    const counter = document.createElement("dodge-counter"); 
    // For TODO R4
    counter.textContent = "0"; 
    counter.style.position = "fixed"; 
    counter.style.top = "10px"; 
    counter.style.right = "10px"; 
    counter.style.padding = "10px"; 
    counter.style.background = "black"; counter.style.color = "white"; 
    counter.style.zIndex = "999999"; document.body.appendChild(counter); 
    window.addEventListener("mousemove", (event) => { 
      // Get the bounding rectangles of the danger zone and the button. 
      const zoneRect = zone.getBoundingClientRect(); 
      const buttonRect = button.getBoundingClientRect(); 
      // Check if the mouse is within 50px of the button's bounding box. 
      if ( event.clientX >= buttonRect.left - 50 && 
        event.clientX <= buttonRect.right + 50 && 
        event.clientY >= buttonRect.top - 50 && 
        event.clientY <= buttonRect.bottom + 50) 
        { counter.textContent = String(Number(counter.textContent) + 1); 
          const maxLeft = zoneRect.width - buttonRect.width;
           const maxTop = zoneRect.height - buttonRect.height; 
           // If the zone is too small to move the button, do nothing. 
           if (maxLeft < 0 || maxTop < 0) {
             return;
             } 
             const oldLeft = buttonRect.left - zoneRect.left; 
             const oldTop = buttonRect.top - zoneRect.top; 
             let newLeft; let newTop; let attempts = 0; 
             // Generate a random position within the danger zone, ensuring the button stays fully 
             // inside, and does not overlap its previous position. 
             do { newLeft = Math.random() * maxLeft; newTop = Math.random() * maxTop; attempts++; } 
             while ( attempts < 100 && 
              newLeft < oldLeft + buttonRect.width && 
              newLeft + buttonRect.width > oldLeft && 
              newTop < oldTop + buttonRect.height && 
              newTop + buttonRect.height > oldTop ); 
              button.style.position = "absolute"; 
              button.style.left = `${newLeft}px`; 
              button.style.top = `${newTop}px`; } 
              // TODO R5: your creative twist. one behavior of your own design.
              //  I intend to make the cursor invisible within the danger zone, 
              // so the user can't see where the button is. 
              if (zone.matches(":hover")) { 
                document.body.style.cursor = "none"; } 
                else { document.body.style.cursor = "default"; } }); })();

// Code is not perfect - in interest of time I am moving on

```

- **How do you remove the portal's original click handler without reloading?**

  > By cloning the button, it allows the button element to still be visible while also removing the event listener attached to the button.

- **How do you stop a keyboard user from triggering the button?**

  > By adding a EventListener with a "keydown", with the event being "event.key === 'Tab'", It allows me to effectively do whatever I want to the "tab" keystroke. I could have a pop-up, but I decided to simply make it not work.

- **How do you keep the button fully inside `#danger-zone` and off its previous position?**

  > By defining bounds using zoneRect and ButtonRect, I am able to create bounds defining where the button is allowed to be (inside the danger zone) and where the button is not allowed to be (its previous position).

## Creativity: my twist, R5

> I made it such that the button is not visible within the danger zone at all. Not only will the user not be able to click the button, but they won't even know when they are close.

## Think like a defender

The mouse trick is theater. The real problem is that attacker code ran in the operator's page at all. If "Purge All Incidents" were a real, destructive action:

1. Where must the actual protection live?

   > At the highest point of authority, exclusively on the developer-facing side. Those who can purge all incidents should be limited to those who can actually fix said incidents.

2. What should the server check on every purge request? Name at least two things.

   > Role(s) of the user requesting it, and number of incidents being purged. 

3. Which Unit 1.3 slide or takeaway does this map to?

   > Real security enforcement must occur server-side, with the frontend used only to support usability and interaction

## Documentation log

| https://medium.com/@theredwillows/moving-an-element-with-javascript-part-1-765c6a083d45 | Moving an Element with JavaScript |
|https://www.w3schools.com/jsref/event_clientx.asp|How to define bounds in JavaScript|
| https://www.w3schools.com/js/js_htmldom_nodes.asp | How to create and manipulate elements in JavScript|
