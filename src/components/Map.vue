<template>
    <div class="map">
        <h3>Карта офиса</h3>

        <div
            v-if="!isLoading"
            class="map-root"
        >
            <MapSVG 
                ref="svg"
                @click="showEmployeeInfo"
            />
            <TableSVG 
                v-show="false" 
                ref="table"
            />
        </div>
        <div v-else>Loading...</div>
    </div>
</template>

<script>
import MapSVG from '@/assets/images/map.svg'
import TableSVG from '@/assets/images/workPlace.svg'
import * as d3 from 'd3'
import tables from '@/assets/data/tables.json'
import legend from '@/assets/data/legend.json'
import people from '@/assets/data/people.json'

export default {
    data() {
        return {
            isLoading: false,
            svg: null,
            g: null,
            tables: [],
            tableSVG: null,
            people: null,
            legend: null
        };
    },
    components: {
        MapSVG,
        TableSVG
    },
    mounted() {
        this.svg = d3.select(this.$refs.svg)
        this.g = this.svg.select('g')
        this.tables = tables
        this.tableSVG = d3.select(this.$refs.table)
        this.legend = legend
        this.people = people

        if(this.g) {
            this.drowTables()
        } else {
            console.log('ERROR')
        }
    },
    methods: {
        drowTables() {
            const svgTablesGroup = this.g.append('g').classed('groupPlacese', true)

            this.tables.map((table) => {
                const svgTable = svgTablesGroup
                    .append('g')
                    .attr('transform', `translate(${table.x}, ${table.y}), scale(0.5)`)
                    .attr('id', table._id)
                    .classed('employer-place', true)

                svgTable
                    .append('g')
                    .attr('transform', `rotate(${table.rotate || 0})`)
                    .attr('group_id', table._id)
                    .html(this.tableSVG.html())
                    .attr(
                        'fill',
                        legend.find((it) => it.group_id === table.group_id)?.color
                        ??
                        'transparent'
                    )

                
            })
        },
        showEmployeeInfo(event) {
            const employeeId = event.target.closest('.employer-place').id
            if (employeeId) {
                const info = this.people.find(employee => employee._id = employeeId)
                console.log(info)
            }
        }
    }
};
</script>

<style scoped>
.map {
    height: 100%;
    width: 100%;
    padding: 24px;
    overflow: hidden;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
}

.map-root {
    height: 100%;
    width: 100%;
    overflow: hidden;
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

::v-deep svg {
    height: 100%;
    width: 100%;
}

::v-deep .table {
    cursor: pointer;
}
</style>
