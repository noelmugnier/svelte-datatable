<script>
  import DTable from "./Table/DTable.svelte";
  import DHeaders from "./Table/DHeaders.svelte";
  import DBody from "./Table/DBody.svelte";
  import DRow from "./Table/DRow.svelte";
  import DCell from "./Table/DCell.svelte";
  import DRowDetails from "./Table/DRowDetails.svelte";
  import DHead from "./Table/DHead.svelte";
  import DCaption from "./Table/DCaption.svelte";
  import DFilters from "./Table/DFilters.svelte";
  import DSelector from "./Table/DSelector.svelte";
  import DExpander from "./Table/DExpander.svelte";
  import DSelectAll from "./Table/DSelectAll.svelte";
  import DExpandAll from "./Table/DExpandAll.svelte";
  import DHell from "./Table/DHell.svelte";
  import DColumnsSelector from "./Table/DColumnsSelector.svelte";
  import DClearFilters from "./Table/DClearFilters.svelte";
  import DApplyFilters from "./Table/DApplyFilters.svelte";
  import DToggleColumnFilters from "./Table/DToggleColumnFilters.svelte";
  import DToggleAdvancedFilters from "./Table/DToggleAdvancedFilters.svelte";
  import DFilter from "./Table/DFilter.svelte";
  import DPaginate from "./Table/DPaginate.svelte";
  import DFoot from "./Table/DFoot.svelte";
  import { onMount } from "svelte";

  let instance = null;
  let description = null;
  let filters = {
    id: null,
    desc: null
  };

  let cursor = null;

  let internalRows = [
    {
      "id": "60be57f7686a61def37adfc8",
      "name": "ipsum tempor",
      "description": "Duis ad ullamco reprehenderit est nulla in cupidatat eu. Consequat exercitation est enim excepteur proident nulla consequat labore quis consectetur consectetur nostrud nisi est. Est veniam mollit amet Lorem veniam velit consequat magna.",
      "createdOn": new Date(),
      "region": "Savannah",
      "state": "Illinois",
      "archived": true
    },
    {
      "id": "60be57f79d3990e46ed2bfda",
      "name": "laborum minim",
      "description": "Culpa ipsum Lorem amet exercitation irure culpa duis cillum adipisicing proident ea elit. Nostrud ut dolor anim amet incididunt reprehenderit sint exercitation eiusmod ipsum est ipsum tempor reprehenderit. Ad laborum aliquip occaecat in amet pariatur laboris cillum minim aute non.",
      "createdOn": new Date(),
      "region": "Sperryville",
      "state": "California",
      "archived": true
    },
    {
      "id": "60be57f7721f117bac1d04c1",
      "name": "et cupidatat",
      "description": "Occaecat commodo aliqua qui enim cillum irure. Esse sunt esse sint officia. Sint ea sint consectetur esse ut elit fugiat amet velit veniam mollit labore elit.",
      "createdOn": new Date(),
      "region": "Kirk",
      "state": "South Dakota",
      "archived": false
    },
    {
      "id": "60be57f7aa0a94d87f2912eb",
      "name": "Lorem aliqua",
      "description": "Reprehenderit pariatur anim duis incididunt. Veniam commodo ea sunt eiusmod excepteur proident amet aliqua ad tempor occaecat est qui. Tempor occaecat laborum reprehenderit esse aliqua.",
      "createdOn": new Date(),
      "region": "Draper",
      "state": "Louisiana",
      "archived": false
    },
    {
      "id": "60be57f7ef10fc25b2e91980",
      "name": "dolore in",
      "description": "Consequat dolor dolor fugiat nostrud laborum minim ad non nulla veniam excepteur aliqua do. Sint dolor officia do fugiat aute. Mollit cillum officia pariatur duis ipsum labore voluptate non sit duis culpa nulla.",
      "createdOn": new Date(),
      "region": "Disautel",
      "state": "Colorado",
      "archived": false
    },
    {
      "id": "60be57f7fff3619a332d6797",
      "name": "duis consectetur",
      "description": "Anim fugiat quis nisi laborum ut incididunt laboris voluptate do pariatur sit incididunt. Voluptate non officia aliqua eiusmod aliquip ea nostrud labore mollit irure in. Fugiat irure anim deserunt sunt.",
      "createdOn": new Date(),
      "region": "Foscoe",
      "state": "Nebraska",
      "archived": true
    },
    {
      "id": "60be57f77e942068032ff252",
      "name": "duis labore",
      "description": "Irure laborum sunt dolore deserunt nostrud culpa nisi magna occaecat ullamco et quis culpa cupidatat. Deserunt sunt veniam adipisicing esse aliquip adipisicing fugiat commodo in eu commodo. Laborum amet sint aliqua mollit.",
      "createdOn": new Date(),
      "region": "Zarephath",
      "state": "Wisconsin",
      "archived": true
    },
    {
      "id": "60be57f777df64bd6489b9c8",
      "name": "minim irure",
      "description": "Pariatur culpa est pariatur qui ex aliqua occaecat. Ea dolor aute aute in ea fugiat proident ullamco. Sunt quis reprehenderit voluptate in adipisicing excepteur ex elit non dolore.",
      "createdOn": new Date(),
      "region": "Emerald",
      "state": "Minnesota",
      "archived": false
    },
    {
      "id": "60be57f75a769ed44a892cc3",
      "name": "in eiusmod",
      "description": "Fugiat reprehenderit et veniam culpa voluptate fugiat nulla excepteur. Sint anim commodo excepteur in eu. Proident veniam quis sit deserunt enim do qui qui qui laboris magna.",
      "createdOn": new Date(),
      "region": "Grantville",
      "state": "Mississippi",
      "archived": true
    },
    {
      "id": "60be57f74e7126b3c69d5f75",
      "name": "amet occaecat",
      "description": "Nostrud laboris mollit nisi amet consequat. Ex laboris irure nostrud tempor laboris ex ipsum tempor cupidatat id ad cupidatat sunt dolore. Enim dolore veniam consectetur elit tempor veniam nulla veniam pariatur aliqua.",
      "createdOn": new Date(),
      "region": "Downsville",
      "state": "Delaware",
      "archived": false
    },
    {
      "id": "60be57f7d7b7b855670828bf",
      "name": "laboris eu",
      "description": "Nulla labore ex deserunt sit reprehenderit pariatur irure nulla voluptate. Magna cupidatat culpa elit sunt Lorem velit ex consectetur qui ex sint. Mollit excepteur nulla occaecat aliquip ipsum reprehenderit in irure nostrud.",
      "createdOn": new Date(),
      "region": "Elbert",
      "state": "Marshall Islands",
      "archived": true
    },
    {
      "id": "60be57f7235cb95c02a8fcf7",
      "name": "cupidatat voluptate",
      "description": "Culpa anim voluptate velit aute dolore nulla. Nostrud exercitation quis pariatur culpa labore in nostrud do veniam. Do quis nostrud culpa ullamco cillum quis irure laboris sit nisi velit irure elit.",
      "createdOn": new Date(),
      "region": "Hackneyville",
      "state": "Rhode Island",
      "archived": false
    },
    {
      "id": "60be57f7f0c86118a05dcba7",
      "name": "veniam velit",
      "description": "Deserunt do ipsum pariatur veniam adipisicing culpa quis. Ut officia culpa do ipsum ex sint est minim fugiat occaecat cupidatat ullamco. Cillum id ea adipisicing reprehenderit incididunt Lorem eu aute nulla culpa minim ut.",
      "createdOn": new Date(),
      "region": "Woodlake",
      "state": "Washington",
      "archived": false
    },
    {
      "id": "60be57f7caf6b93a47001e14",
      "name": "cillum et",
      "description": "Veniam voluptate occaecat elit Lorem elit ea in adipisicing sit amet cupidatat. Proident veniam ex do est. Pariatur eu quis nulla minim magna consectetur ullamco.",
      "createdOn": new Date(),
      "region": "Shasta",
      "state": "Ohio",
      "archived": true
    },
    {
      "id": "60be57f78d3649db2eb6b54a",
      "name": "consequat non",
      "description": "Pariatur ut voluptate velit velit anim duis enim. Nostrud dolor nisi esse voluptate nulla ipsum eiusmod ullamco. Quis ipsum deserunt tempor voluptate pariatur cillum reprehenderit sunt labore.",
      "createdOn": new Date(),
      "region": "Tecolotito",
      "state": "Texas",
      "archived": false
    },
    {
      "id": "60be57f782feefef59ca5dce",
      "name": "anim veniam",
      "description": "Aute Lorem ea cillum aliqua laboris ut consectetur laborum amet ut cillum irure. Amet velit et enim sunt deserunt irure ex eu qui fugiat tempor commodo. Tempor consectetur commodo anim commodo deserunt eu.",
      "createdOn": new Date(),
      "region": "Vicksburg",
      "state": "Northern Mariana Islands",
      "archived": false
    },
    {
      "id": "60be57f77d2866c673d6feeb",
      "name": "ullamco fugiat",
      "description": "Aliquip laboris velit in irure duis sint laboris minim quis excepteur eiusmod sit. Velit labore eu deserunt sunt consequat fugiat velit ad culpa aliqua. Irure ut ex nulla occaecat commodo esse.",
      "createdOn": new Date(),
      "region": "Grahamtown",
      "state": "West Virginia",
      "archived": true
    },
    {
      "id": "60be57f77fb7b745d916a5e2",
      "name": "duis est",
      "description": "Culpa deserunt sunt dolore ex do in laborum sint reprehenderit officia exercitation. Veniam esse labore eiusmod et deserunt dolore Lorem duis occaecat veniam in mollit. Excepteur nisi occaecat exercitation et incididunt commodo proident dolor.",
      "createdOn": new Date(),
      "region": "Carrizo",
      "state": "North Carolina",
      "archived": false
    },
    {
      "id": "60be57f72a88f3de918e2d8a",
      "name": "consectetur aliquip",
      "description": "Laborum irure culpa sunt adipisicing ea commodo mollit. Irure mollit deserunt ullamco dolor. Nostrud ex est amet officia elit Lorem voluptate nulla nostrud.",
      "createdOn": new Date(),
      "region": "Ticonderoga",
      "state": "American Samoa",
      "archived": false
    },
    {
      "id": "60be57f7e8f1476add50e46b",
      "name": "culpa elit",
      "description": "Ullamco culpa anim labore aliqua nulla sunt aliqua duis sunt veniam laboris tempor. Officia id id Lorem Lorem do cupidatat est fugiat labore anim duis sint. Irure qui nulla in minim excepteur.",
      "createdOn": new Date(),
      "region": "Roulette",
      "state": "Vermont",
      "archived": false
    },
    {
      "id": "60be57f72adf68e064caf051",
      "name": "minim in",
      "description": "Ipsum dolore Lorem quis sint pariatur occaecat Lorem sint adipisicing fugiat irure labore voluptate minim. Deserunt veniam consequat commodo laboris qui reprehenderit. Cupidatat cupidatat id eiusmod deserunt cupidatat.",
      "createdOn": new Date(),
      "region": "Wanship",
      "state": "Alaska",
      "archived": true
    },
    {
      "id": "60be57f7f9446dd4a5dda35d",
      "name": "aliquip veniam",
      "description": "Magna quis ea non voluptate duis esse. Nisi dolore nulla ullamco elit laboris occaecat ad proident deserunt eu proident veniam. Nulla irure ea commodo non.",
      "createdOn": new Date(),
      "region": "Virgie",
      "state": "Federated States Of Micronesia",
      "archived": true
    },
    {
      "id": "60be57f77f93022444bb6868",
      "name": "nostrud exercitation",
      "description": "Pariatur sunt duis sint laborum ea ipsum velit sunt anim excepteur. Exercitation ad ea aliqua exercitation id aliquip commodo reprehenderit. Aute deserunt velit fugiat dolor amet aute tempor dolore.",
      "createdOn": new Date(),
      "region": "Monument",
      "state": "Massachusetts",
      "archived": true
    },
    {
      "id": "60be57f7e84ef6980e032a7b",
      "name": "est commodo",
      "description": "Dolor officia incididunt officia magna id deserunt pariatur Lorem consequat aliqua excepteur ea sint est. Mollit non deserunt anim ut enim non. Deserunt ea non ipsum incididunt ipsum deserunt elit amet enim.",
      "createdOn": new Date(),
      "region": "Coyote",
      "state": "Nevada",
      "archived": true
    },
    {
      "id": "60be57f78e5f0c5bcdab667c",
      "name": "nisi duis",
      "description": "Incididunt tempor consequat culpa mollit proident exercitation velit tempor. Cillum anim velit voluptate irure elit minim proident proident labore ea esse deserunt quis. Qui eiusmod deserunt ipsum cillum aliqua.",
      "createdOn": new Date(),
      "region": "Sexton",
      "state": "Pennsylvania",
      "archived": true
    },
    {
      "id": "60be57f7e1406eea6ba81d98",
      "name": "ea irure",
      "description": "Ex culpa sunt ullamco incididunt nostrud consectetur in consectetur laborum et. Cillum commodo exercitation fugiat anim aliqua qui. Eiusmod ut ullamco duis sunt sit culpa aliqua cupidatat quis consectetur.",
      "createdOn": new Date(),
      "region": "Yukon",
      "state": "Kentucky",
      "archived": true
    },
    {
      "id": "60be57f7ad3ccd3729edd46e",
      "name": "irure laboris",
      "description": "Labore fugiat in consequat deserunt esse. Exercitation dolore officia amet est adipisicing laborum esse ullamco eiusmod officia consequat anim. Officia laborum aliqua ea amet officia.",
      "createdOn": new Date(),
      "region": "Beaulieu",
      "state": "Tennessee",
      "archived": true
    },
    {
      "id": "60be57f7fd829fe5015d001e",
      "name": "eiusmod aute",
      "description": "Minim duis deserunt culpa est ullamco quis tempor tempor dolore amet reprehenderit adipisicing dolore voluptate. Pariatur incididunt reprehenderit occaecat irure sunt eu laborum commodo. Veniam occaecat ipsum sunt pariatur irure dolor tempor commodo do tempor labore.",
      "createdOn": new Date(),
      "region": "Shawmut",
      "state": "Alabama",
      "archived": false
    },
    {
      "id": "60be57f72e0b9a6c73ba637a",
      "name": "consectetur ullamco",
      "description": "Culpa esse reprehenderit ad excepteur cupidatat ut id commodo id. Qui voluptate commodo cupidatat est adipisicing ullamco. Ullamco excepteur in amet quis consectetur id id qui.",
      "createdOn": new Date(),
      "region": "Northchase",
      "state": "Palau",
      "archived": false
    },
    {
      "id": "60be57f7014099a01836dc8f",
      "name": "ea tempor",
      "description": "Anim tempor mollit dolor aute officia magna proident commodo mollit. Et dolor nostrud cupidatat irure dolore. Velit et anim ut veniam.",
      "createdOn": new Date(),
      "region": "Winesburg",
      "state": "North Dakota",
      "archived": false
    },
    {
      "id": "60be57f78044feb56fad103f",
      "name": "in in",
      "description": "Anim cupidatat voluptate ad velit ad adipisicing minim duis eiusmod ipsum. Cupidatat veniam voluptate irure est in veniam adipisicing veniam commodo anim velit aliqua proident. Id magna enim consequat minim officia qui labore eu ut.",
      "createdOn": new Date(),
      "region": "Allendale",
      "state": "Utah",
      "archived": true
    },
    {
      "id": "60be57f706f9832f00276018",
      "name": "non elit",
      "description": "Dolore ad minim quis sunt amet nisi non adipisicing officia adipisicing. Amet nisi mollit voluptate ipsum amet nostrud est. Quis culpa ex labore consectetur deserunt consectetur consectetur aliquip dolore nostrud Lorem.",
      "createdOn": new Date(),
      "region": "Waikele",
      "state": "Missouri",
      "archived": false
    },
    {
      "id": "60be57f78fd0f567d326bbaa",
      "name": "in exercitation",
      "description": "Velit anim aliqua deserunt eiusmod deserunt ad est est commodo aliqua nisi cillum. Amet cupidatat adipisicing ipsum incididunt adipisicing culpa commodo laborum consequat consectetur culpa. Dolore sunt occaecat adipisicing laboris aliqua duis eu.",
      "createdOn": new Date(),
      "region": "Yogaville",
      "state": "Virgin Islands",
      "archived": false
    },
    {
      "id": "60be57f7df47cb4d04a2771d",
      "name": "ex pariatur",
      "description": "Aliqua id id ad tempor consectetur culpa tempor eu voluptate dolor nostrud sunt ut culpa. Sunt et excepteur officia et labore voluptate quis ullamco magna minim aliqua labore. Sint ipsum sit fugiat velit cupidatat anim qui ea cupidatat ipsum nisi ullamco dolore.",
      "createdOn": new Date(),
      "region": "Catherine",
      "state": "New Hampshire",
      "archived": false
    },
    {
      "id": "60be57f7fe721d7a9e9c2683",
      "name": "ut aliquip",
      "description": "Voluptate enim minim incididunt id ipsum. Cupidatat aute fugiat eiusmod ex consectetur aliquip duis nostrud commodo. Amet sunt id est ut.",
      "createdOn": new Date(),
      "region": "Saddlebrooke",
      "state": "Kansas",
      "archived": true
    }
  ];

  function* getNextStateId() {
    let i = 0;

    while (true) {
      yield i++;
    }
  }
  function* getNextRegionId() {
    let i = 0;

    while (true) {
      yield i++;
    }
  }
  
  const stateIds = getNextStateId();
  const regionIds = getNextRegionId();

  let rows = [];
  let states = [{ value: null, label: "" }];

  let regions = [{ value: null, label: "" }];
  let selectedStates = [];

  const updateDescriptionFilter = (field) => {
    instance.updateFilters(field, description);
  };

  const updateStateFilter = (field) => {
    instance.updateFilters(field, selectedStates);
  };

  const clearAdvancedFilters = (filters) => {
    description = filters["description"];
    states = filters["states"];
  };

  const refresh = (e) => {
    cursor = cursor ? cursor++ : e.detail.pagination.cursor ?? 1;
    let page = (e.detail.pagination.page ?? 0) * e.detail.pagination.take;
    rows = internalRows.slice(page, page + e.detail.pagination.take);
  };
  
  onMount(() => {
    internalRows.map(r => {
      states.push({ value: stateIds.next().value, label: r.state });
      regions.push({ value: regionIds.next().value, label: r.region });
    });
    
    states = states;
    regions = regions;
  })
</script>

<DTable bind:instance on:refresh={refresh}>
  <DCaption>
    Super table
    <DToggleColumnFilters />
    <DToggleAdvancedFilters />
    <DColumnsSelector />
  </DCaption>
  <DHead>
    <DHeaders>
      <DSelectAll />
      <DExpandAll multi={false} />
<!--      <DHell field="id" sort filter type="int" display="Identifiant" />-->
      <DHell field="name" sort filter type="string" display="Nom" />
      <DHell field="createdOn" sort filter type="daterange" hidden display="Crée le" />
      <DHell field="region" filter type="select" display="Région"
             values={regions} />
      <DHell field="archived" sort filter type="boolean"
             values={[{value: true, label: "OK"}, {value:false, label:"KO"}]} display="Archivé" />
      <DHell size={2}>
        <DClearFilters on:clear={clearAdvancedFilters} />
        <DApplyFilters on:apply={(e) => console.log(e.detail)} />
      </DHell>
    </DHeaders>
    <DFilters>
      <DFilter field="description">
        <label>Description contains</label>
        <input type="text" on:change={(e) => updateDescriptionFilter("description")} bind:value={description}
               placeholder="type some text" />

        <label>In state</label>
        <select multiple on:change={(e) => updateStateFilter("state")} >
          {#each states as state}
            <option value={state.value}>{state.label}</option>
          {/each}
        </select>
      </DFilter>
    </DFilters>
  </DHead>
  <DBody {rows} let:row rowIdentifier="id">
    <DRow {row} on:click={() => console.log('row clicked')}>
      <DSelector on:select={(e) => console.log(`row ${e.detail.identifier} selected: ${e.detail.selected}`)} />
      <DExpander on:expand={(e) => console.log(`row ${e.detail.identifier} expanded: ${e.detail.expanded}`)} />
<!--      <DCell field="id">{row.id}</DCell>-->
      <DCell field="name">{row.name}</DCell>
      <DCell field="createdOn">{row.createdOn.toLocaleDateString()}</DCell>
      <DCell field="region">{row.region === 1 ? 'Ain' : 'Isère'}</DCell>
      <DCell field="archived">{row.archived}</DCell>
      <DCell>
        <button on:click|stopPropagation={() => console.log('update')}>Update</button>
        <button on:click|stopPropagation={() => console.log('remove')}>Remove</button>
      </DCell>
      <DCell>
        <button on:click|stopPropagation={() => console.log('assign')}>Assign</button>
      </DCell>
    </DRow>
    <DRowDetails {row}>
      <p>Identifiant: <strong>{row.id}</strong></p>
      <p>State:{row.state}</p>
      <p>Description: {row.description}</p>
    </DRowDetails>
  </DBody>
  <DFoot>
    <DPaginate showFirst={true} showLast={true} totalRows={internalRows.length} />
    <!--    <DInfinite {cursor}/>-->
  </DFoot>
</DTable>