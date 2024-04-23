<script>
    import { onMount } from "svelte";
    import { DataTable } from "simple-datatables";
    import Modal from "../components/modal.svelte";
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

    let loadingData = true;

    let showModal = false;

    function closeModal() {
        showModal = false;
    }

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
                result.hostname = res.map(
                    (data) => new URL(atob(data.link)).hostname,
                );
                result.links = res.map((data) => atob(data.link));
            });
        //remove duplocate hostname
        result.hostname = [...new Set(result.hostname)];
        return result;
    }

    /**
     * @type {{
     * domain: string,
     *  hostname: string[],
     *  judi: number
     * links: string[]
     * }}
     */
    let selectedData;

    /**
     * @type string
     */
    let modalMessage;

    /**
     * @type {string}
     */
    let modalTitle;

    /**
     * @type {string}
     */
    let modalSubTitle;

    /**
     * @param {number} judi
     */
    const jumlahJudiLabel = (judi) => {
        if (judi >= 99) {
            return (
                "<span class='text-red-500 font-bold'>" +
                judi.toString() +
                "+</span>"
            );
        } else if (judi >= 50) {
            return (
                "<span class='text-orange-500 font-bold'>" +
                judi.toString() +
                "+</span>"
            );
        } else if (judi > 0) {
            return (
                "<span class='text-yellow-500 font-bold'>" +
                judi.toString() +
                "</span>"
            );
        } else {
            return (
                "<span class='text-green-500 font-bold'>" +
                judi.toString() +
                "</span>"
            );
        }
    };

    /**
     * @param {number} host
     */
    const jumlahHostnameLabel = (host) => {
        if (host >= 10) {
            return (
                "<span class='text-red-500 font-bold'>" +
                host.toString() +
                "</span>"
            );
        } else if (host >= 5) {
            return (
                "<span class='text-orange-500 font-bold'>" +
                host.toString() +
                "+</span>"
            );
        } else if (host > 0) {
            return (
                "<span class='text-yellow-500 font-bold'>" +
                host.toString() +
                "</span>"
            );
        } else {
            return (
                "<span class='text-green-500 font-bold'>" +
                host.toString() +
                "</span>"
            );
        }
    };

    let datatable;

    const initDatatable = () => {
        datatable = new DataTable("#myTable", {
            searchable: true,
            fixedHeight: true,
            fixedColumns: true,
            paging: true,
            perPage: 100,
            perPageSelect: [5, 10, 20, 50, 100],
        });
    };

    const loadData = async () => {
        for (let pemprov of arrayPemprov) {
            await fecthData(pemprov).then((data) => {
                datas.push(data);
            });
        }
        //await 1 second
        await new Promise((resolve) => setTimeout(resolve, 1000));
        datas = datas;
        loadingData = false;
    };

    onMount(async () => {
        await loadData();
        initDatatable();
        /**
         * @type {string[][]}
         */
        let newData = [];
        datas.forEach((data, i) => {
            newData.push([
                "<a href='https://www.google.com/search?q=site%3A" +
                    data.domain +
                    "+%22slot+online%22+OR+site%3A" +
                    data.domain +
                    "+%22gacor%22%27&num=100' class='underline'>" +
                    data.domain +
                    "</a>",
                "<span class='font-bold open-judi cursor-pointer' data-index='" +
                    i.toString() +
                    "'>" +
                    jumlahJudiLabel(data.judi) +
                    "</span>",
                "<span class='font-bold open-domain cursor-pointer' data-index='" +
                    i.toString() +
                    "'>" +
                    jumlahHostnameLabel(data.hostname.length) +
                    "</span>",
            ]);
        });
        datatable.insert({
            headings: [
                "Website URL",
                "Jumlah Situs Judi (Perkiraan)",
                "Jumlah Domain / Subdomain",
            ],
            data: newData,
        });

        //add click event to .open-domain
        document.querySelectorAll("span.open-domain").forEach((element) => {
            element.addEventListener("click", (e) => {
                modalTitle = "Domain yang disusupi";
                modalSubTitle =
                    "bagian yang disusupi bisa berupa komentar, hidden link, atau bagian yang datanya dinamis";
                selectedData =
                    datas[parseInt(e.currentTarget.getAttribute("data-index"))];
                let lists = "<ol class='list-decimal'>";
                for (
                    let index = 0;
                    index < selectedData.hostname.length;
                    index++
                ) {
                    const hostname = selectedData.hostname[index];
                    lists += "<li>" + hostname + "</li>";
                }
                lists += "</ol>";
                modalMessage = lists;
                showModal = true;
            });
        });

        //add click event to .open-judi
        document.querySelectorAll("span.open-judi").forEach((element) => {
            element.addEventListener("click", (e) => {
                modalTitle = "Halaman yang disusupi";
                modalSubTitle =
                    "bagian yang disusupi bisa berupa komentar, hidden link, atau bagian yang datanya dinamis";
                selectedData =
                    datas[parseInt(e.currentTarget.getAttribute("data-index"))];
                let lists = "<ol class='list-decimal'>";
                for (
                    let index = 0;
                    index < selectedData.links.length;
                    index++
                ) {
                    const link = selectedData.links[index];
                    lists += "<li>" + link + "</li>";
                }
                lists += "</ol>";
                modalMessage = lists;
                showModal = true;
            });
        });

        //escape key to close modal
        document.addEventListener("keydown", (e) => {
            if (e.key === "Escape") {
                showModal = false;
            }
        });
    });
</script>

<svelte:head>
    <title>Pemprov Yang Paling Gacor!</title>
    <link
        href="https://cdn.jsdelivr.net/npm/simple-datatables@latest/dist/style.css"
        rel="stylesheet"
        type="text/css"
    />
</svelte:head>

<div class="text-gray-200 w-full p-6">
    {#if showModal}
        <Modal
            title={modalTitle}
            message={modalMessage}
            subTitle={modalSubTitle}
            on:close={closeModal}
        />
    {/if}

    <div class="dark flex flex-col w-full min-h-screen p-4 md:p-6 text-white">
        <header class="flex flex-col mb-4">
            <h1 class="text-2xl font-bold">Pemprov Yang Paling Gacor!</h1>
            <p>
                Website Pemerintah Provinsi Yang Terindikasi Disusupi Situs Judi
            </p>
            <p>
                Berdasarkan google dork <span class="underline italic"
                    >site:nama_provinsi.go.id "slot online" OR
                    site:nama_provinsi.go.id "gacor"</span
                >
            </p>
            <p class="text-yellow-400">
                list ini sifatnya tidak realtime, bisa saja halaman yang ter
                index google sudah di fix
            </p>
            <p class="text-yellow-400">
                Terakhir diupdate tanggal : 23/04/2024
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
            <div class="w-full overflow-x-auto min-h-96">
                {#if loadingData}
                    <div class="flex justify-center items-center w-full">
                        <div
                            class="animate-spin rounded-full h-32 w-32 border-b-2 border-gray-100"
                        ></div>
                    </div>
                {:else}
                    <table id="myTable">
                        <thead>
                            <tr>
                                <th>Website</th>
                                <th>Link yang disusupi (Perkiraan)</th>
                                <th>Jumlah domain / subdomain</th>
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
                {/if}
            </div>
        </div>
    </div>
</div>
