<script>
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
                        {#each arrayPemprov as pemprov}
                        <tr
                            class="border-b transition-colors hover:bg-muted/50 data-[state=selected]:bg-muted"
                        >
                            <td class="px-4 py-4 text-left align-middle">
                                <a
                                    href="https://{pemprov}"
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    class="text-blue-400 hover:underline"
                                >
                                    {pemprov}
                                </a>
                            </td>
                            <td class="px-4 py-4 text-left align-middle">
                                {Math.floor(Math.random() * 100)}
                            </td>
                            <td class="px-4 py-4 text-left align-middle">
                                {Math.floor(Math.random() * 100)}
                            </td>
                        </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
