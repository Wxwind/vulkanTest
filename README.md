1. openGL 只关心clip space 和 viewport space，均为左手坐标系（x朝右，y朝上，z朝屏幕里), framebuffer中x朝右y朝上，
而window坐标是x朝右y朝下（原点在左下角），所以opengl会在映射到屏幕上时做一个y轴反转的操作。vulkan的clip space/view space/framebuffer均为x朝右y朝下
详情见[stackoverflow](https://stackoverflow.com/questions/4124041/is-opengl-coordinate-system-left-handed-or-right-handed/12336360#12336360)上的解释
2. glm rotate是右手坐标系。 lookAt可以用 GLM_FORCE_LEFT_HANDED 宏设置成左手，否则默认(view space)是右手