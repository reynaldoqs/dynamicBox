<template>
<div class="drag-container">
    <div ref='mask' :style="maskStyles">
        <drag-mask />
    </div>
    <div ref='dragElement' :class="{'default-bg':true, 'opened': isOpened && !isExpanded}" :style="elementStyles">
        <drag-element :isOpened="isOpened" :isExpanded="isExpanded" @expandBox="isExpanded = true" @compressBox="isExpanded = false" @openBox="isOpened = true" @closeBox="isOpened = false">
            <slot slot='element'/>
        </drag-element>
    </div>
</div>
</template>

<script>
import DragMask from './DragMask';
import DragElement from './DragElement'
export default {
    name: 'aud-draggable',
    data() {
        return {
            isOpened: false,
            isExpanded: false,
            maskStyles: {
                width: null,
                height: null
            },
            elementStyles: {
                position: 'absolute',
                top: 0,
                left: 0
            },
            draggableElement: null,
            dragElements: {
                startX: null,
                startY: null,
                initialMouseX: null,
                initialMouseY: null
            },
            targetSize: {
                width: null,
                height: null
            },
            limitPadding: 0,
            openedPadding: 10,
            viewportW: window.innerWidth || 0,
            viewportH: window.innerHeight || 0
        }
    },
    props: {
        openedW: Number,
        openedH: Number,
        sticky: Boolean,
        draggable: Boolean,
        animate: Boolean,
        openedPosition: {
            validator(value) {
                return ['top left', 'top right', 'bottom left', 'bottom right'].indexOf(value) !== -1
            }
        }
    },
    watch: {
        isOpened(val) {
            if (val) {
                this.$nextTick(() => {
                    this.$set(this.elementStyles, 'position', 'fixed')
                    this.moveComponentTo(this.calTargetPos().left, this.calTargetPos().top)
                    this.doItDraggable()
                })
            } else {
                this.isExpanded = false
                this.dismissDragable()
            }
        },
        isExpanded(val) {
            if (val) {
                if (this.draggableElement.requestFullscreen) {
                    this.draggableElement.requestFullscreen();
                } else if (this.draggableElement.mozRequestFullScreen) {
                    this.draggableElement.mozRequestFullScreen();
                } else if (this.draggableElement.webkitRequestFullscreen) {
                    this.draggableElement.webkitRequestFullscreen();
                } else if (this.draggableElement.msRequestFullscreen) {
                    this.draggableElement.msRequestFullscreen();
                }
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                } else if (document.mozCancelFullScreen) {
                    document.mozCancelFullScreen();
                } else if (document.webkitExitFullscreen) {
                    document.webkitExitFullscreen();
                } else if (document.msExitFullscreen) {
                    document.msExitFullscreen();
                }
            }
        }
    },
    mounted() {

        this.draggableElement = this.$refs.dragElement;
        this.initSizesConfig()
        this.$nextTick(() => {
            this.doItSticky()
        })
    },
    methods: {
        initSizesConfig() {
            const {
                width,
                height
            } = this.draggableElement.getBoundingClientRect()
            //set mask container to preserver our size
            this.$set(this.maskStyles, 'width', width + 'px')
            this.$set(this.maskStyles, 'height', height + 'px')
            //cal and set our target size for expand and open box
            this.$set(this.targetSize, 'width', this.openedW ? this.openedW : width)
            this.$set(this.targetSize, 'height', this.openedH ? this.openedH : height)

        },
        doItDraggable() {
            if (!this.draggable) return
            if (this.draggableElement) {
                this.draggableElement.addEventListener('mousedown', this.handleDragMousedown)
            }
        },
        handleDragMouseup(event) {
            document.removeEventListener('mousemove', this.handleDragMousemove)
            document.removeEventListener('mouseup', this.handleDragMouseup)
            event.preventDefault()
        },
        handleDragMousemove(event) {
            const movePosition = (pos, start, size, view, padding) => {
                if ((start + pos) <= padding) {
                    return padding
                } else if ((start + pos + size) > (view - padding)) {
                    return view - size - padding
                } else {
                    return (start + pos)
                }
            }
            let dx = event.clientX - this.dragElements.initialMouseX
            let dy = event.clientY - this.dragElements.initialMouseY
            let leftPos = movePosition(dx, this.dragElements.startX, this.draggableElement.scrollWidth, this.viewportW, this.limitPadding)
            let topPos = movePosition(dy, this.dragElements.startY, this.draggableElement.scrollHeight, this.viewportH, this.limitPadding)
            this.$set(this.elementStyles, 'left', leftPos + 'px')
            this.$set(this.elementStyles, 'top', topPos + 'px')
            event.preventDefault()
        },
        handleDragMousedown(event) {

            this.dragElements.startX = this.draggableElement.offsetLeft
            this.dragElements.startY = this.draggableElement.offsetTop
            this.dragElements.initialMouseX = event.clientX
            this.dragElements.initialMouseY = event.clientY
            document.addEventListener('mousemove', this.handleDragMousemove)
            document.addEventListener('mouseup', this.handleDragMouseup)
        },
        dismissDragable() {
            if (this.draggableElement) {
                this.$set(this.elementStyles, 'position', 'absolute')
                this.$set(this.elementStyles, 'left', '0px')
                this.$set(this.elementStyles, 'top', '0px')
                this.$set(this.elementStyles, 'width', this.maskStyles.width)
                this.$set(this.elementStyles, 'height', this.maskStyles.height)
                this.draggableElement.removeEventListener('mousedown', this.handleDragMousedown)
            }
        },
        moveComponentTo(targetLeft = 20, targetTop = 20) {
            const {
                top,
                left,
                width,
                height
            } = this.draggableElement.getBoundingClientRect()
            if (this.animate) {
                this.draggableElement.animate([{
                        left: left + 'px',
                        top: top + 'px',
                        width: width + 'px',
                        height: height + 'px',
                        opacity: 0
                    },
                    {
                        left: targetLeft + 'px',
                        top: targetTop + 'px',
                        width: this.targetSize.width + 'px',
                        height: this.targetSize.height + 'px',
                        opacity: 1
                    },
                ], {
                    duration: 200,
                    easing: 'ease-out',
                    fill: 'none'
                })
            }
            this.$set(this.elementStyles, 'top', targetTop + 'px')
            this.$set(this.elementStyles, 'left', targetLeft + 'px')
            this.$set(this.elementStyles, 'width', this.targetSize.width + 'px')
            this.$set(this.elementStyles, 'height', this.targetSize.height + 'px')
        },
        doItSticky() {
            if (!this.sticky) return
            console.log('is sticky');
            let sL = 0
            const findScrollableElement = (el) => {
                try {
                    do {
                        el = el.parentNode
                    } while (!el.hasAttribute('scrollable'))
                    return el
                } catch {
                    return window
                }
            }
            const calComulativeTop = (el) => {
                let top = 0
                do {
                    top += el.offsetTop || 0
                    el = el.offsetParent
                } while (el);
                return top
            }
            const scrollHandler = () => {
                const currentScroll = scrollEl === window ? scrollEl.scrollY : scrollEl.scrollTop
                if ((currentScroll - height - topOffset) > 0 && isGoingDown(currentScroll)) {
                    this.isOpened = true
                }
                if (currentScroll-topOffset <= 0) {
                    this.isOpened = false
                }
            }
            const isGoingDown = (scroll) => {
                const direction = sL < scroll ? true : false
                sL = scroll
                return direction
            }
            const scrollEl = findScrollableElement(this.$el)
            const topOffset = calComulativeTop(scrollEl)

            const {
                height
            } = this.$el.getBoundingClientRect()
            scrollEl.addEventListener('scroll', scrollHandler)
        },
        calTargetPos() {
            switch (this.openedPosition) {
                case 'top left':
                    return {
                        left: this.openedPadding, top: this.openedPadding
                    }
                    case 'top right':
                        return {
                            left: this.viewportW - this.targetSize.width - this.openedPadding, top: this.openedPadding
                        }
                        case 'bottom left':
                            return {
                                left: this.openedPadding, top: this.viewportH - this.targetSize.height - this.openedPadding
                            }
                            case 'bottom right':
                                return {
                                    left: this.viewportW - this.targetSize.width - this.openedPadding, top: this.viewportH - this.targetSize.height - this.openedPadding
                                }
                                default:
                                    return {
                                        left: this.viewportW - this.targetSize.width - this.openedPadding, top: this.openedPadding
                                    }
            }
        }
    },
    components: {
        DragElement,
        DragMask
    }
}
</script>

<style scoped>
.default-bg * {
    box-sizing: border-box;
}
.drag-container * {
    user-select: none;
}
.drag-container {
    display: inline-block;
    position: relative;
}

.default-bg {
    background-color: #313131;
}
.opened{
    box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
    border-radius: 4px;
    overflow: hidden;
}
</style>
