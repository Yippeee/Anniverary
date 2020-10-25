.<template>
<div class="img-wrap">
    <img src="../assets/logo.png" class="trace-img" />
    <div class="info-box">
        <!-- times and location -->
        <span class="time">2018-06-22</span>
        <span class="location">成都</span>
    </div>
</div>
</template>

<script>
export default {
    setup(props) {
        console.log(1)
    }
}
</script>

<style lang="less">
.img-wrap {
    display: inline-flex;
    position: relative;
    height: 173px;
    width: 109px;
    overflow: hidden;

    &:hover {
        .info-box {
            z-index: 3;

        }
    }

    .trace-img {
        position: absolute;
        width: 100%;
        height: 100%;
        object-fit: contain;
        cursor: pointer;
        transition: all 0.5s;
        z-index: 2;
    }

    .info-box {
        position: absolute;
        display: flex;
        justify-content: center;
        width: 100%;
        height: 100%;
        background-color: white;
        z-index: 1;

    }
}
</style>
