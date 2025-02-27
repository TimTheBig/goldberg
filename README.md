# Goldberg
**Goldberg** is a Rust procedural macro library for obfuscating Rust code. Its obfuscation techniques are designed to survive both compilation as well as optimization. While not intended to be a source code obfuscator, it *can* be used as such. It is named after Rube Goldberg machines.

Currently, the following types of obfuscation are supported:

* code-flow obfuscation
* string literal encryption
* integer literal obfuscation

The documentation can be found [here](https://docs.rs/goldberg). For usage examples, read [the tests file](https://github.com/frank2/goldberg/blob/main/tests/tests.rs). The changelog history can be found [here](https://github.com/frank2/goldberg/blob/main/CHANGELOG.md).

```rs
use goldberg::goldberg_stmts;

let result = goldberg_stmts! {
   {
      fn print(value: u32) {
         let msg = String::from("value:");
         println!("{} {}", msg, value);
      }

      let mut x: u32 = 0xDEADBEEFu32;
      print(x);
      x ^= 0xFACEBABEu32;
      print(x);
      x ^= 0xDEFACED1u32;
      print(x);
      x ^= 0xABAD1DEAu32;
      print(x);
      x
   }
};

assert_eq!(result, 0x5134d76a);
```

This example expands into code similar to this:
```rs
fn print(value: u32) {
    let msg = String::from(
        {
            let key_fgnliibu: Vec<u8> = vec![75u8, 87u8, 169u8, 234u8, 230u8, 38u8];
            let mut string_hkzmkgaw: Vec<u8> = vec![61u8, 54u8, 197u8, 159u8, 131u8, 28u8];
            for pulhfjddcbiztuxz in 0..string_hkzmkgaw.len() {
                string_hkzmkgaw[pulhfjddcbiztuxz] ^= key_fgnliibu[pulhfjddcbiztuxz];
            }
            String::from_utf8(string_hkzmkgaw).unwrap()
        }
        .as_str(),
    );
    println!("{} {}", msg, value);
}
struct _AssertDefault_cfygodkf
where
    u32: Default;
let mut x: u32 = u32::default();
let mut ident_gqtkhobp = 1113386507u32;
let mut key_ftudpieg = 0u32;
'loop_obfu_jmcfjvhq: loop {
    match ident_gqtkhobp {
        2158235392u32 => {
            print(x);
            key_ftudpieg = 3044081204u32;
        }
        2506875858u32 => {
            x ^= {
                struct _AssertDefault_vedfwrhy
                where
                    u32: Default;
                let mut calc_whsuusro: u32 = u32::default();
                let mut ident_tmheadmi = 1821101871u32;
                let mut key_pzediytf = 0u32;
                'loop_obfu_msqcffqh: loop {
                    match ident_tmheadmi {
                        1103538895u32 => {
                            calc_whsuusro ^= 2534362044u32;
                            key_pzediytf = 2755681459u32;
                        }
                        3757011920u32 => {
                            calc_whsuusro = calc_whsuusro.swap_bytes();
                            key_pzediytf = 849856391u32;
                        }
                        1071321848u32 => {
                            calc_whsuusro = calc_whsuusro.rotate_left(1692640787u32);
                            key_pzediytf = 1375898541u32;
                        }
                        ...
```
