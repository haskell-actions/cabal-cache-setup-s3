# cabal-cache-setup-s3
Sets up cabal-cache to store Haskell packages cache on S3

When set up, provides `cabal-cache-store` and `cabal-cache-restore` commands.

## Usage example

```yaml
- uses: haskell-actions/cabal-cache-setup-s3@v1
  with:
    awsKey: ${{ secrets.BINARY_CACHE_ACCESS_KEY_ID }}
    awsSecretKey: ${{ secrets.BINARY_CACHE_SECRET_ACCESS_KEY}}
    awsRegion: ${{ secrets.BINARY_CACHE_REGION }}
    archiveUri: ${{ secrets.BINARY_CACHE_URI }}
    cabalStorePath: ${{ steps.setup-haskell.outputs.cabal-store }}
    threadsNum: 8

- name: Configure
  run: cabal configure --write-ghc-environment-files=ghc8.4.4+

- name: Restore binary cache
  run: cabal-cache-restore

- name: Build
  run: cabal build all

- name: Save binary cache
  if: ${{ always() }}
  run: cabal-cache-store
  ```
