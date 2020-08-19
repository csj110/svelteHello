<script>
  import Board from '../components/Board.svelte';
  let board = [
    {
      id: 1,
      name: 'TODO',
      items: [
        { id: 1, name: 'item41' },
        { id: 2, name: 'item42' },
        { id: 3, name: 'item43' },
        { id: 4, name: 'item44' },
        { id: 5, name: 'item45' },
        { id: 6, name: 'item46' },
        { id: 7, name: 'item47' },
        { id: 8, name: 'item48' },
        { id: 9, name: 'item49' },
      ],
    },
    {
      id: 2,
      name: 'DOING',
      items: [],
    },
    {
      id: 3,
      name: 'DONE',
      items: [],
    },
  ];
</script>

<Board columnItems="{board}"/>

<style>
  :global(*) {
    box-sizing: border-box;
    margin: 0;
  }
</style>
