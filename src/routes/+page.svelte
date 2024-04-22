<script>
    import { onMount } from "svelte";
    import { DataTable } from "simple-datatables";
    const arrayPemprov = [
        "acehprov.go.id",
        "baliprov.go.id",
        "bantenprov.go.id",
        "bengkuluprov.go.id",
        "jogjaprov.go.id",
        "jakarta.go.id",
        "gorontaloprov.go.id",
        "jambiprov.go.id",
        "jabarprov.go.id",
        "jatengprov.go.id",
        "jatimprov.go.id",
        "kalbarprov.go.id",
        "kalselprov.go.id",
        "kalteng.go.id",
        "kaltimprov.go.id",
        "kaltaraprov.go.id",
        "babelprov.go.id",
        "kepriprov.go.id",
        "lampungprov.go.id",
        "malukuprov.go.id",
        "malutprov.go.id",
        "ntbprov.go.id",
        "nttprov.go.id",
        "papua.go.id",
        "papuabaratprov.go.id",
        "papuabaratdayaprov.go.id",
        "papuapegunungan.go.id",
        "papuaselatan.go.id",
        "papuatengahprov.go.id",
        "riau.go.id",
        "sulbarprov.go.id",
        "sulselprov.go.id",
        "sultengprov.go.id",
        "sultraprov.go.id",
        "sulutprov.go.id",
        "sumbarprov.go.id",
        "sumselprov.go.id",
        "sumutprov.go.id",
    ];

    /**
     * @type {{
     * domain: string,
     *  hostname: string[],
     *  judi: number
     * links: string[]
     * }[]}
     */
    let datas = [];

    /**
     * Fetch data from API
     * @param {string} pemprov
     */
    async function fecthData(pemprov) {
        /**
         * @type {{
         * domain: string,
         *  hostname: string[],
         *  judi: number
         * links: string[]
         * }}
         */
        let result = {
            domain: pemprov,
            hostname: [],
            judi: 0,
            links: [],
        };
        await fetch(`/data/${pemprov}.json`)
            .then((response) => response.json())
            .then((res) => {
               
                result.judi = res.length;
                result.hostname = res.map((data) => new URL(data.link).hostname);
                result.links = res.map((data) => data.link);
            });
        //remove duplocate hostname
        result.hostname = [...new Set(result.hostname)];
        return result;
    }

    onMount(async () => {
        for (let pemprov of arrayPemprov) {
            datas.push(await fecthData(pemprov));
        }
        datas = datas;
        const datatable = new DataTable("#myTable", {
            searchable: true,
            fixedHeight: true,
            fixedColumns: true,
            paging: true,
            perPage: 100,
            perPageSelect: [5, 10, 20, 50, 100],
        });
        /**
         * @type {string[][]}
         */
        let newData = [];
        datas.forEach((data) => {
            newData.push([data.domain, data.judi.toString(), data.hostname.length.toString()]);
        });
        datatable.insert({
            headings: ["Website URL", "Jumlah Situs Judi (Perkiraan)", "Jumlah Domain / Subdomain"],
            data: newData,
        });
    });
</script>

<svelte:head>
    <title>Pemprov Yang Paling Gacor!</title>
    <link href="https://cdn.jsdelivr.net/npm/simple-datatables@latest/dist/style.css" rel="stylesheet" type="text/css">
</svelte:head>

<div class="text-gray-200 w-full p-6">
    <div class="dark flex flex-col w-full min-h-screen p-4 md:p-6 text-white">
        <header class="flex flex-col mb-6">
            <h1 class="text-2xl font-bold">Pemprov Yang Paling Gacor!</h1>
            <p>
                Website Pemerintah Provinsi Yang Terindikasi Disusupi Situs Judi
            </p>
        </header>
        <!-- <div class="flex flex-col gap-4 md:flex-row md:gap-6">
            <div class="w-full md:w-1/3">
                <input
                    class="flex h-10 rounded-md border border-input px-3 py-2 text-sm ring-offset-background file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 w-full bg-gray-700 text-white placeholder-gray-400"
                    placeholder="Search by name or status"
                />
            </div>
        </div> -->
        <div class="mt-6">
            <div class="w-full overflow-x-auto">
                <table  id="myTable">
                    <thead>
                        <tr>
                            <th>Website URL</th>
                            <th>Jumlah Situs Judi (Perkiraan)</th>
                            <th>Jumlah Domain / Subdomain</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- {#each datas as data}
                            <tr>
                                <td>
                                    <a href={data.domain} target="_blank" rel="noopener noreferrer" class="text-blue-400 hover:underline">{data.domain}</a>
                                </td>
                                <td>{data.judi}</td>
                                <td>{data.hostname.length}</td>
                            </tr>
                        {/each} -->
                    </tbody>
                </table>
                
            </div>
        </div>
    </div>
</div>
