<template>
    <div class="container">
        <h1 v-if="!finished">Turn player <span class="player"> {{ player}}  </span> </h1>
        <h1 v-else-if="finished === 'win'" class="winner">Winner <span class="player"> {{ player}} </span> </h1>
        <h1 v-else class="winner"><span class="player"> Draw </span> </h1>
        <div class="game-table" :style="{'pointer-events': running !== -1 ? 'none' : 'auto'}">
            <Square 
                v-for="(val, index) in squares" 
                :key="index" 
                :index="index" 
                :mark="squares[index]" 
                v-on:mark="onClick"
            />
        </div>
        <div class="actions">
            <button @click="restart" class="btn-restart">Restart</button>
        </div>
    </div>
</template>


<script lang="ts">
import Vue from 'vue';
import { Component } from 'vue-property-decorator';
import Square from './Square.vue';

@Component({
    components: {
        Square,
    },
})
export default class GameTable extends Vue {

    private player: string = 'X';
    private squares: any[] = Array(9).fill(null);
    private finished: boolean | string = false;
    private running: number = -1;

    get areAllMarked() {
        return this.squares.every((a) => a);
    }

    public setMark(i: number) {
        if (!this.isMarked(i)) {
            this.squares.splice(i, 1, this.player);
        }
    }

    public isMarked(i: number): boolean {
        return this.squares[i] !== null;
    }

    public changeTurn() {
        const matchResult = this.matchResult();

        if (matchResult) {
            this.finished = matchResult;
        } else {
            if (this.player === 'X') {
                this.player = 'O';
                this.machineMove();
            } else {
                clearTimeout(this.running);
                this.running = -1;
                this.player = 'X';
            }
        }
    }

    public onClick(i: number): void {
        if (this.finished) return;
        this.play(i);
    }

    public play(i: number): void {
        this.setMark(i);
        this.changeTurn();
    }

    public machineMove(): void {
        const markPoint = Math.floor(Math.random() * 9);

        if (!this.isMarked(markPoint)) {
            this.wait(markPoint, this.onClick);
        } else if (this.areAllMarked) {
            return;
        } else {
            return this.machineMove();
        }
    }

    public wait(args: any, cb: (i: any) => void): void {
        this.running = setTimeout(() => {
            cb(args);
        }, 100);
    }

    public matchResult() {
         if (this.checkWinner()) {
            return 'win';
        } else if (this.isDraw()) {
            return 'draw';
        }
        return '';
    }

    public isDraw(): boolean {
        return this.areAllMarked && !this.finished && !this.checkWinner();
    }

    public checkWinner(): boolean {
        return (this.checkRows() || this.checkCols() || this.checkDiagonals());
    }

    public checkRows(): boolean {
        return (this.checkPos(0, 1, 2) || this.checkPos(3, 4, 5) || this.checkPos(6, 7, 8));
    }

    public checkCols(): boolean {
        return (this.checkPos(0, 3, 6) || this.checkPos(1, 4, 7) || this.checkPos(2, 5, 8));
    }

    public checkDiagonals(): boolean {
        return (this.checkPos(0, 4, 8) || this.checkPos(2, 4, 6));
    }

    public checkPos(n: number, n1: number, n2: number): boolean {
        return !!this.squares[n] && this.squares[n] === this.squares[n1] && this.squares[n1] === this.squares[n2];
    }

    public restart(): void {
        this.squares = Array(9).fill(null);
        this.finished = false;
        this.player = 'X';
        clearTimeout(this.running);
        this.running = -1;
    }
}
</script>

<style scoped>
    .game-table {
        display: grid;
        grid-template-columns: repeat(3, 100px);
        max-width: 300px;
        margin: 80px auto;
    }

    .btn-restart {
        background: transparent;
        color: inherit;
        font-size: 20px;
        border: 2px solid #fff;
        cursor: pointer;
        outline: none;
        padding: 5px 20px;
    } 

    .winner {
        transform: scale(1.7);
    }

    .player {
        border: 2px solid;
        padding: 5px;
    }
</style>
