```css
const styles = StyleSheet.create({

  buttonContainer: {

    width: 320,

    height: 68,

    marginHorizontal: 20,

    alignItems: 'center',//vertically align

    justifyContent: 'center',//horizontally align

    padding: 3,

  },

  button: {

    borderRadius: 10,

    width: '100%', // pressable area is 100%

    height: '100%',// pressable area is 100%

    alignItems: 'center',

    justifyContent: 'center',

    flexDirection: 'row',// align child items horizontally from left to right

  },

  buttonLabel: {

    color: '#fff',

    fontSize: 16,

  },

});

```
```tsx
import { Pressable, StyleSheet, Text, View } from 'react-native';

  

type Props = {

  label: string;

};

  

export default function Button({ label }: Props) {

  return (

    <View style={styles.buttonContainer}>

      <Pressable style={styles.button} onPress={() => alert('You pressed a button.')}>

        <Text style={styles.buttonLabel}>{label}</Text>

      </Pressable>

    </View>

  );

}
```
[[ViewDrawing]]