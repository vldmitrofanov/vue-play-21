<template>
  <div>
    <b-navbar toggleable="lg" type="light" variant="light">
      <div class="container">
        <b-navbar-brand href="/">
          <div class="logo">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="2500"
              height="2158"
              viewBox="0 0 256 221"
              preserveAspectRatio="xMinYMin meet"
            >
              <path
                d="M204.8 0H256L128 220.8 0 0h97.92L128 51.2 157.44 0h47.36z"
                fill="#41B883"
              />
              <path
                d="M0 0l128 220.8L256 0h-51.2L128 132.48 50.56 0H0z"
                fill="#41B883"
              />
              <path
                d="M50.56 0L128 133.12 204.8 0h-47.36L128 51.2 97.92 0H50.56z"
                fill="#35495E"
              />
            </svg>
          </div>
          <span class="brand-title">vue-play-21</span>
        </b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-item href="#" @click.stop.prevent="showHelpModal = true"
              >Rules</b-nav-item
            >
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
    <b-modal v-model="showHelpModal" scrollable title="21 Bet" hide-footer>
      <h3>Introduction</h3>
      <p>
        21 Bet is a variation with a modified decks and fixed drawing rules
        between the players. It is also known as Russian Blackjack and Ochko. In
        this variant of the game you play with the dealer as your opponent.
      </p>
      <h3>Rules</h3>
      <ol>
        <li>
          A 36-card deck is used, containing only ranks six to ace, four suits
          per rank.
        </li>
        <li>
          Cards are valued as follows:
          <ul>
            <li>6 to 10 — Pip value</li>
            <li>Ace — 11 points</li>
            <li>Jack — 2 points</li>
            <li>Queen — 3 points</li>
            <li>King — 4 points</li>
          </ul>
        </li>
        <li>
          As in blackjack, the hand with more points, without going over 21,
          wins.
        </li>
        <li>As in blackjack, a busted hand loses.</li>
        <li>There are two hands, both of which have fixed drawing rules.</li>
        <li>The dealer shall give each hand two cards initially.</li>
        <li>
          In this variation of the game there is no "Golden Point" (which
          consists of two aces).
          <em>It might be added in the future releases.</em>
        </li>
        <li>
          If in your hand less than 17 points, the dealer shall give you a card.
          If that card causes you to bust, then your opponent shall win.
          Otherwise, go onto the rule 9.
        </li>
        <li>
          If your opponent has less than 17 points, the dealer shall give that
          hand a card. If that card causes your opponent to bust, then you shall
          win. Otherwise, go onto the rule 10.
        </li>
        <li>
          If at least one hand has under 17 points and four or less cards, then
          go back rule 8.
        </li>
      </ol>
    </b-modal>
  </div>
</template>
<script>
export default {
  data() {
    return {
      showHelpModal: false,
    };
  },
};
</script>
