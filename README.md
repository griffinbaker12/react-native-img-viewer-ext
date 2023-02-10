# react-native-img-viewer-ext

this project is an extension of: https://github.com/ascoders/react-native-image-viewer.

that version only allowed for swipe down to close, so I extended it to allow for swipe up to close as well.

An example of using this component is thus:

```js
import { useEffect, useState } from "react";
import { View, Text, Modal, Dimensions, Pressable, TouchableOpacity } from 'react-native'
import { useSafeAreaInsets } from "react-native-safe-area-context";
import { EvilIcons } from '@expo/vector-icons';
import { ImageViewer as _ImageViewer } from 'react-native-img-viewer-ext';
import ProfileImage from "./ProfileImage";
import Loading from "./Loading";

interface Props {
    url: string;
    setImage: (url: string) => void;
}

const Header = ({ top, right, handleClose }: { right: number; top: number; handleClose: () => void }) => {
    const iconSize = 28;

    // then if you want to add an animation that moves it down when it slides in you can, and that also adjusts when the image moves as discussed below then we can do that as well

    return (
        <TouchableOpacity style={{ top: top + 16, zIndex: 1 }} onPress={handleClose}>
            <EvilIcons name="close" size={iconSize} color="white" style={{ right: right - iconSize / 2, position: 'absolute' }} />
        </TouchableOpacity>
    )
}

const ImageViewer = ({ url, setImage }: Props) => {
    const insets = useSafeAreaInsets();

    const [visible, setVisible] = useState(true);

    const width = Dimensions.get('window').width;
    const size = width * 0.8;
    const right = (width - size) / 2;

    const handleClose = () => {
        setVisible(false);
        setImage('');
    }

    return (
        <Modal animationType="fade" visible={visible} transparent={true} >
            <_ImageViewer enableSwipeUp onSwipeUp={handleClose} swipeDownThreshold={110} swipeUpThreshold={110} renderHeader={() => <Header handleClose={handleClose} right={right} top={insets.top} />} loadingRender={() => <Loading />} renderIndicator={() => <></>} onSwipeDown={handleClose} enableSwipeDown useNativeDriver={true} imageUrls={[{ url }]} renderImage={(props: any) => <View {...props} style={{ flex: 1, alignSelf: 'center', justifyContent: 'center', alignItems: 'center' }}>
                {/* whatever img component you want */}
                <ProfileImage uri={url} size={size} type="community" />
            </View>} />
        </Modal>
    )
}

export default ImageViewer;
```