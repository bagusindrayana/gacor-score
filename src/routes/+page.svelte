<script>
    import { onMount } from "svelte";
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
    });
</script>

<div class="text-gray-200 w-full p-6">
    <div class="dark flex flex-col w-full min-h-screen p-4 md:p-6 text-white">
        <header class="flex flex-col mb-6">
            <h1 class="text-2xl font-bold">Pemprov Yang Paling Gacor!</h1>
            <p>
                Website Pemerintah Provinsi Yang Terindikasi Disusupi Situs Judi
            </p>
        </header>
        <div class="flex flex-col gap-4 md:flex-row md:gap-6">
            <div class="w-full md:w-1/3">
                <input
                    class="flex h-10 rounded-md border border-input px-3 py-2 text-sm ring-offset-background file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 w-full bg-gray-700 text-white placeholder-gray-400"
                    placeholder="Search by name or status"
                />
            </div>
        </div>
        <div class="mt-6">
            <div class="relative w-full overflow-auto">
                <table class="w-full caption-bottom text-sm text-gray-300">
                    <thead class="[&amp;_tr]:border-b">
                        <tr
                            class="border-b transition-colors hover:bg-muted/50 data-[state=selected]:bg-muted"
                        >
                            <th
                                class="h-12 px-4 text-left align-middle font-medium text-muted-foreground [&amp;:has([role=checkbox])]:pr-0"
                            >
                                Website URL
                            </th>
                            <th
                                class="h-12 px-4 text-left align-middle font-medium text-muted-foreground [&amp;:has([role=checkbox])]:pr-0"
                            >
                                Jumlah Situs Judi (Perkiraan)
                            </th>
                            <th
                                class="h-12 px-4 text-left align-middle font-medium text-muted-foreground [&amp;:has([role=checkbox])]:pr-0"
                            >
                                Jumlah Domain/Subdomain
                            </th>
                        </tr>
                    </thead>
                    <tbody class="[&amp;_tr:last-child]:border-0">
                        {#each datas as data}
                            <tr
                                class="border-b transition-colors hover:bg-muted/50 data-[state=selected]:bg-muted"
                            >
                                <td class="px-4 py-4 text-left align-middle">
                                    <a
                                        href={data.domain}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        class="text-blue-400 hover:underline"
                                    >
                                        {data.domain}
                                    </a>
                                </td>
                                <td class="px-4 py-4 text-left align-middle">
                                    {data.judi}
                                </td>
                                <td class="px-4 py-4 text-left align-middle">
                                    {data.hostname.length}
                                </td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
